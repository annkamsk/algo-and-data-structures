# Kopiec

Struktura danych oparta na drzewie (zwykle binarnym).

Kopiec definiuje *własność kopca* czyli relacja, którą utrzymuje każdy wierzchołek ze swoimi dziećmi. Np. dla relacji mniejszości własnością będzie: każdy rodzic musi być mniejszy od swoich dzieci.

Kopiec binarny łatwo zaimplementować w tablicy: indeksując tablicę od 1, dzieci wierzchołka przechowywanego pod indeksem `i` znajdować się będą pod indeksami `2*i` i `2*i+1`. Jego rodzic znajdować się będzie pod indeksem `i/2`.  

### Przywracanie własności kopca

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

### Budowa kopca

```python
def buildHeap(A):
  for i in range(len(A)/2-1, 0, -1):
    heapify(A, i)
```

Złożoność: O(n)


