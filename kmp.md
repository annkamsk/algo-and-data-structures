# Algorytm Knutha-Morissa-Pratta

Algorytm wyszukiwania wzorca w tekście.

### Tablica częściowych dopasowań

Jest to tablica, w której pod każdym indeksem i zapisujemy długość najdłuższego właściwego prefiksu wzorca będącego również jego sufiksem występującego w podsłowie od 0 do i-1.	

Właściwy prefiks – prefiks, który nie jest jednocześnie całym słowem.

Przykład: dla wzorca W = "ABCDABD" tablica T będzie wyglądać następująco:

| i    | 0    | 1    | 2    | 3    | 4    | 5    | 6    |
| ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- |
| W[i] | A    | B    | C    | D    | A    | B    | D    |
| T[i] | -1   | 0    | 0    | 0    | 0    | 1    | 2    |

T[0] zawsze inicjalizujemy na -1, bo nie ma słów przed indeksem 0. Dla i = 5 podsłowem jest "ABCDA", który ma prefikso-sufiks długości 1: "A". Dla i = 6 podsłowem jest "ABCDAB", a prefikso-sufiksem jest "AB".

### Wyszukiwanie

```python
def KMP_search(text, pattern):
  T = create_T(pattern)
  
  id_t = 0 # current position in text
  id_p = 0 # current position in pattern
  while id_t + 1 < len(text):
    if pattern[id_p] == text[id_t+1]:
      id_p += 1
      if id_p == len(pattern): # we found a full match
        return id_t
    else:
      id_t = id_t + id_p - T[id_p]
      if id_p > 0:
        id_p = T[id_p]
 return len(text)
```

Złożoność jest liniowa względem długości tekstu. 
