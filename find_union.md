# Find–Union

To struktura danych, która przechowuje dla ustalonego zbioru jego podział na mniejsze, rozłączne zbiory. Struktura taka umożliwia dwie operacje:

* Find: Wyznacza, w którym zbiorze jest dany element, pozwalając na sprawdzenie, czy dwa elementy są w tym samym zbiorze.
* Union: Łączy dwa zbiory w jeden.

Zwykle jest implementowana jako las drzew.

```python
def make_set(x):
  x.parent = None

def find(x):
  if x.parent == None
  	return x
  return find(x.parent)

def union(x, y):
  x_root = find(x)
  y_root = find(y)
  if x_root != y_root:
    x_root.parent = y_root
```

Tworzone w ten sposób drzewa mogą być bardzo niezrównoważone i operacja Find może mieć złożoność O(n).

Pierwszym usprawnieniem jest *łączenie według rangi*, które polega na dołączaniu mniejszego drzewa do większego drzewa według rangi. Ranga to wartość przechowywana w wierzchołku:

* pojedynczy element ma rangę 0
* kiedy łączymy dwa drzewa o równej randze r, to ranga powstałego drzewa wynosi r+1

```python
def make_set(x):
	x.parent = None
  x.rank = 0
 
def union(x, y):
  x_root = find(x)
  y_root = find(y)
  if x_root.rank > y_root.rank:
  	y_root.parent = x_root
  elif x_root.rank < y_root.rank:
    x_root.parent = y_root
  elif x_root != y_root:
    y_root.parent = x_root
    x_root.rank += 1
```

Drugie udoskonalenie, zwane kompresją ścieżki, polega na spłaszczaniu struktur drzewa zawsze, kiedy używamy Find. Każdy węzeł, który napotykamy na naszej drodze podczas szukania korzenia jest natychmiast przepinany tak, aby korzeń był jego rodzicem.

```python
def find(x):
  if x.parent == None
  	return x
  x.parent = find(x.parent)
  return x.parent
```

Obie techniki stosowane łącznie powodują, że łączny czas m operacji Find i Union jest O(m&alpha;(m, n)), gdzie n jest liczbą elementów uniwersum, zaś &alpha;  bardzo wolno rosnącą odwrotnością funkcji Ackermanna (we wszystkich praktycznych sytuacjach &alpha; (m,n) < 6).

Złożoność pesymistyczna (niezamortyzowana): O(log n)
