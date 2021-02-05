# Drzewo AVL

Zrównoważone drzewo BST, w którym wysokość prawego i lewego poddrzewa różni się co najwyżej o 1. 

Zrównoważenie drzewa osiąga się, przypisując każdemu węzłowi współczynnik zrównoważenia, który jest równy różnicy wysokości lewego i prawego poddrzewa. Jeśli po wstawieniu lub usunięciu wierzchołka przyjmie on wartość spoza {-1, 0, 1}, wykonuje się specjalną operację rotacji węzłów, która przywraca zrównoważenie.

Współczynnik zrównoważenia(x) = Height(x.right) - Height(x.left)

Pesymistyczny czas wyszukiwania, wstawiania i usuwania: O(log n)

Drzewo AVL o wysokości h zawiera co najmniej F_{h+2} wierzchołków, gdzie F_n jest n-tą liczbą Fibonacciego.

W celu zrównoważenia przeprowadzamy takie same rotacje jak dla drzewa [Splay](splay.md).

Niech x będzie wierzchołkiem, który aktualnie ma współczynnik zrównoważenia równy 2 lub -2. Niech z będzie jego dzieckiem z wyższym poddrzewem. 

Możliwe sytuacje:

* z jest prawym dzieckiem x i WZ(z) >= 0 &#8594; single_rotation(z)
* z jest lewym dzieckiem x i WZ(z) <= 0 &#8594; single_rotation(z)
* z jest prawym dzieckiem x i WZ(z) < 0 &#8594; double_rotation_opposite_directions(z)
* z jest lewym dzieckiem z i WZ(z) > 0 &#8594; double_rotation_opposite_directions(z)

 
