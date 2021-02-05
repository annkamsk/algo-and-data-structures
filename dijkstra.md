# Algorytm Dijkstry

Algorytm służy do znajdowania najkrótszej ścieżki z pojedynczej źródła w grafie o nieujemnych wagach krawędzi.

Algorytm Dijkstry znajduje w grafie wszystkie najkrótsze ścieżki pomiędzy wybranym wierzchołkiem a wszystkimi pozostałymi, przy okazji wyliczając również koszt przejścia każdej z tych ścieżek.

Niech s – wierzchołek źródłowy, w(i, j) – waga krawędzi (i, j), d – tablica odległości od źródła dla wszystkich wierzchołków, na początku d[s] = 0 i d[i] = infinity dla wszystkich i != s.

Tworzymy kolejkę priorytetową Q, do której będziemy wrzucać wierzchołki. Priorytetem kolejki jest aktualnie wyliczona odległość od wierzchołka źródłowego.

Dopóki kolejka nie jest pusta, usuwamy z niej wierzchołek u o najniższym priorytecie i dla każdego z jego sąsiadów v aktualizemy priorytet jako min(d[v], d[u] + w(u, v)). 

Na końcu d zawiera wszystkie najkrótsze odległości.

```python
import math

# G is a set of nodes
# Instead of array d we use a .key property on nodes
def dijkstra(G, weights, s):
  Q = Queue()
  for v in G:
    if v != s:
    	Q.insert(v, math.inf)
  Q.insert(s, 0)
  
  while not Q.empty():
    u = Q.min()
    Q.deleteMin()
    for v in u.neighbors:
      Q.decreaseKey(v, min(v.key, u.key+weights(u, v)))
```

Złożoność zależy od implementacji kolejki priorytetowej:

* dla kopca Fibonacciego: O(E + V * log V)
* dla kopca: O(E * log V)

gdzie E – liczba krawędzi, V – liczba wierzchołków
