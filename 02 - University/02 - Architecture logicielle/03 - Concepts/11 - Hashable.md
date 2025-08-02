#### Définition simple :

Un objet **hashable**, c’est un objet immuable avec un **identifiant numérique stable** (son _hash_) qu’on peut utiliser comme clé dans un dictionnaire ou un élément dans un set.

``` python
my_dict = {clé_hashable: valeur} 
my_set = {objet_hashable1, objet_hashable2}
```
#### Comment savoir si un objet est hashable ?

1. A une méthode `__hash__()` qui retourne un entier stable
2. Est **immuable** (son contenu ne change pas une fois créé)
3. Implémente `__eq__()` pour tester l’égalité
#### Mutable vs Immutable

- **Mutable** = peut **changer après création**
- **Immutable** = ne peut **jamais changer** après création

