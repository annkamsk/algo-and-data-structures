# Kolejki priorytetowe

Kolejka priorytetowa to struktura z operacjami:

* Init(): tworzy pustą kolejkę
* Insert(x): wstawia element x do kolejki
* FindMin(): zwraca element o najmniejszym kluczu 
* DelMin(): zwraca element o najmniejszym kluczu, usuwając go z kolejki
* DecreaseKey(x, y): nadaje kluczowi elementu x nową, mniejszą wartość y
* Delete(x): usuwa element x z kolejki

Czasem również dostępna jest operacja:

* Meld(Q1, Q2): zwraca nową kolejkę, będącą połączenie kolejek Q1 i Q2

Najprostszą implementacją kolejki jest zwykła lista. Innymi popularnymi implementacjami są: kopiec binarny, kolejka dwumianowa, kopiec Fibonacciego.

| Operacja | Lista | Kopiec binarny | Kolejka dwumianowa | Kopiec Fibonacciego(*) |
| -------- | ----- | -------------- | ------------------ | ------------------- |
| Init     | 1     | 1              | 1                  | 1                   |
|Insert|1|lg n|lg n|1|
|FindMin|n|1|lg n|1|
|DelMin|n|lg n|lg n|lg n|
|DecreaseKey|1|lg n|lg n|1|
|Delete|1|lg n|lg n|lg n|
|Meld|1|n|lg n|1|

(*) Podane koszty są zamortyzowane.

### Kopiec binarny

Struktura danych oparta na drzewie binarnym.

Kopiec definiuje *własność kopca* czyli relacja, którą utrzymuje każdy wierzchołek ze swoimi dziećmi. Np. dla relacji mniejszości własnością będzie: każdy rodzic musi być mniejszy od swoich dzieci.

Kopiec binarny łatwo zaimplementować w tablicy: indeksując tablicę od 1, dzieci wierzchołka przechowywanego pod indeksem `i` znajdować się będą pod indeksami `2*i` i `2*i+1`. Jego rodzic znajdować się będzie pod indeksem `i/2`.  

##### Przywracanie własności kopca

```python
def heapify(A, i):
  left = 2*(i+1)
  right = 2*(i+1)+1
  maximum = i
  
  # find max out of 3 A[i], A[left], A[right]
  if left < len(A) and A[left] > A[maximum]:
    maximum = left
  if right < len(A) and A[right] > A[maximum]:
    maximum = right
    
  if maximum != i:
    # swap
    max_v = A[maximum]
    A[maximum] = A[i]
    A[i] = max_v
    
    heapify(A, maximum)
```

Złożoność: O(log n)

##### Budowa kopca

```python
def buildHeap(A):
  for i in range(len(A)/2-1, 0, -1):
    heapify(A, i)
```

Złożoność: O(n)

### Kolejka dwumianowa

Kolekcja drzew dwumianowych o rekurencyjnej definicji:
B_0 jest drzewem jednowęzłowym, a korzeń drzewa B_{k+1} ma k+1 dzieci stanowiących korzenie drzew B_0,...,B_k w tej kolejności.

![img](http://smurf.mimuw.edu.pl/sites/default/files/images/Drzewa_dwumianowe.png)

Drzewo dwumianowe B_k:

* ma 2^k węzłów
* wysokość k
* dokładnie bin_coeff(k, i) węzłów na poziomie i (stąd nazwa)
* można otrzymać dołączając do drzewa B_{k-1} drugie drzewo B_{k-1}

##### FindMin

W każdym drzewie wchodzącym w skład kolejki najmniejszy element znajduje się w korzeniu, więc znalezienie minimalnego elementu wymaga przejścia listy drzew.
Koszt: O(lg n)

##### Meld

Łączenie drzew przypomina dodawania liczb binarnych: sumowaniu jedynek na pozycji i w liczbie odpowiada łączenie dwóch drzew dwumianowych stopnia i. Korzeń z mniejszym kluczem zostaje korzeniem wynikowego drzewa, a korzeń z większym kluczem staje się jego prawym dzieckiem. 
Łączenie dwóch kolejek polega więc na przejściu obydwu list drzew i złączeniu drzew jednakowych stopni.
Koszt: O(lg n)

##### Insert

To Meld, w którym druga kolejka jest jednoelementowa.
Koszt: O(lg n)

##### DelMin

Polega na znalezieniu elementu minimalnego, odcięciu od niego listy jego dzieci (która jest poprawną kolejką dwumianową) i wykonaniu Meld na całej kolejce i na niej. 

Koszt: O(lg n)

##### DecreaseKey

Wykonuje się podobnie do poprawiania warunku kopca w kopcu binarnym: zmniejszony klucz wędruje w górę drzewa, zamieniając się ze swoim rodzicem. Maksymalna wysokość drzewa do lg n, więc koszt operacji będzie wynosił O(lg n).

##### Delete

Polega na przesunięciu elementu do korzenia (jak przy DecreaseKey) i usunięciu tego korzenia (jak w DelMin).

Koszt: O(lg n)

### Kopiec Fibonacciego

Kopiec Fibonacciego to kolejka drzew, które spełniają warunek kopca: klucz dziecka jest większy bądź równy kluczowi rodzica.

Drzewa nie muszą być posortowane.

Każdy węzeł ma nie więcej niż O(log n) dzieci, a rozmiar poddrzewa zakorzenionego w węźle stopnia k jest co najmniej F_k+2, gdzie Fk jest k-tą liczbą Fibonacciego.

##### FindMin

Trzymamy wskaźnik do minimalnego elementu.

Złożoność: O(1)

##### Merge

Łączymy obie listy korzeni drzew.

Złożoność: O(1)

##### Insert

Tworzymy nowy kopiec z jednym elementem i dokładamy go do listy. 

Złożoność: O(1)

##### DeleteMin

Usuwamy korzeń z minimum, a jego dzieci dołączamy do listy drzew. 

Następnie szukamy nowego minimum, po drodze łącząc ze sobą drzewa o takich samych stopniach (stopień trzymamy w korzeniu). Łączenie drzew polega na podłączeniu wierzchołka o większym kluczu jako dziecka korzenia drugiego drzewa. 

Aby efektywnie wyszukiwać drzewa o tym samym stopniu tworzymy tablicę wskaźników o długości O(log n) gdzie trzymamy wskaźniki do drzew o kolejnych rzędach. Jeśli rozpatrujemy drzewo o rzędzie k, ale w k-tej komórce tablicy istnieje już wskaźnik, oznacza to ze powinniśmy złączyć oba drzewa i zapisać w komórce k+1.

Koszt (zamortyzowany): O(log n)

##### DecreaseKey

Zaczynamy od zmniejszenia wartości zadanego wierzchołka i w razie gdy nowa wartość zaburza porządek kopcowy wywołujemy operację odcinania rozważanego wierzchołka.

Operacja odcinania wierzchołka x odcina go od jego rodzica i dołącza do listy korzeni kopca. Dodatkowo y zostaje oznaczony jako wierzchołek, który utracił jedno dziecko. Przy próbie odcięcia drugiego dziecka, nie odcinamy go, tylko wywołujemy odcinanie dla rodzica, przenosząc operację w górę drzewa tak długo, aż dojdziemy do korzenia, lub wierzchołka który nie utracił jeszcze dziecka.
