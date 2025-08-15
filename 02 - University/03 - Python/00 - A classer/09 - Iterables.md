t. En Python, `Iterable` est ce qu’on appelle une **interface** au sens conceptuel, même si Python ne définit pas les interfaces comme en Java ou en C#.

Plus précisément, `Iterable` est :

### 1. Une **ABC** (Abstract Base Class)

Elle se trouve dans le module `collections.abc` et aussi dans `typing` pour l’annotation générique.

``` python
from collections.abc import Iterable
# ou
from typing import Iterable
```

### 2. Ce qu’elle impose

Un objet est **considéré comme un `Iterable`** s’il implémente la méthode spéciale `__iter__()`.

``` python
class MonObjet:
    def __iter__(self):
        return iter([1, 2, 3])
```

→ Ici, `MonObjet()` est un `Iterable`, car `__iter__()` est défini.

### 3. iter(\[1, 2, 3])

C’est une fonction **built-in** Python très simple… en surface. Mais en dessous, elle exploite des mécanismes très puissants liés à la **Python Virtual Machine (PVM)** et au **protocol d’itération**.

#### 3.1 Ce que fait concrètement `iter(obj)`

Quand tu fais :
``` python
iter(obj)
```

Python fait **exactement** :

``` python
obj.__iter__()
```

Si l’objet a une méthode `__iter__()`, Python la **considère comme conforme au protocole Iterable**.

Mais si l’objet **n’a pas** `__iter__()`… alors Python regarde :

``` python
obj.__getitem__(index)
```

Et il crée **automatiquement un itérateur** basé sur `obj[0], obj[1], ...` jusqu’à ce que ça lève une `IndexError`.

👉 Donc : tu peux être `Iterable` soit par `__iter__`, soit par `__getitem__`.



