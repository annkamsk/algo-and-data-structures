# Drzewa splay

Drzewo poszukiwań binarnych, w którym wykonanie podstawowych operacji wiąże się z wykonaniem procedury splay(T, x), która uruchamia ciąg rotacji, które mają umieścić wierzchołek x w korzeniu drzewa T.

Nie są samorównoważące się. Ich wysokość może być liniowa.

Złożoności (zamortyzowane):

Search(x) – O(log n)

Insert(x) – O(log n)

Delete(x) – O(log n)



### Rotacje

W czasie procedury Splay(T, x) możemy wykonywać trzy rodzaje rotacji:

#### Pojedyncza lewa/prawa rotacja 

![Splay tree zig.svg](https://upload.wikimedia.org/wikipedia/commons/thumb/2/2c/Splay_tree_zig.svg/709px-Splay_tree_zig.svg.png)



```python
def single_rotation(x):
  p = x.parent
  
  is_left = p.left == x
    
  x.parent = p.parent
  p.parent = x
  
  if is_left:
    p.left = x.right
    p.left.parent = p
    x.right = p
  else:
    p.right = x.left
    p.right.parent = p
    x.left = p
```

#### Podwójna rotacja lewo-lewo lub prawo-prawo

![Zigzig.gif](https://upload.wikimedia.org/wikipedia/commons/f/fd/Zigzig.gif)

```python
def double_rotation_same_direction(x): 
  p = x.parent
  # assert p.left == x and p.parent.left == p) or (p.right == x and p.parent.right == p
  single_rotation(x.parent)
  single_rotation(x)
  
```

#### Podwójna rotacja lewo-prawo lub prawo-lewo

![Zigzag.gif](https://upload.wikimedia.org/wikipedia/commons/6/6f/Zigzag.gif)

```python
def double_rotation_opposite_directions(x):
  p = x.parent
  assert (p.left == x and p.parent.right == p) or (p.right == x and p.parent.left == p)
  single_rotation(x)
  single_rotation(x)
```

```python
def rotate(x):
  # x is a root already
  if x.parent == None:
    return
  p = x.parent
  g = p.parent
  # x's parent is a root
  if g == None:
  	single_rotation(x)
  elif (p.left == x and g.left == p) or (p.right == x and g.right == p):
    double_rotation_same_direction(x)
  else:
    double_rotation_opposite_directions(x)
    
```

### Operacje

#### Search

Jak w drzewie BST i jeśli zakończyło się powodzeniem i znaleźliśmy węzeł x, to wywołujemy Splay(x).

#### Insert

Wykonujemy Search(x). Jeśli w korzeniu znajduje się wartość, którą chcieliśmy wstawić, to kończymy. Jeśli nie, to wkładamy x między korzeń, a jego prawe lub lewe dziecko.

#### Delete

Wykonujemy Search(x). Usuwamy korzeń, a następnie wyszukujemy w jego lewym poddrzewie wartości x, co powoduje przemieszczenie do korzenia tego poddrzewa największej w nim wartości. Podłączamy do niej prawe poddrzewo.


