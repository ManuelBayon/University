### **1️⃣ Annotations de base**

- **Types primitifs** : `int`, `float`, `str`, `bool`, `bytes`
- **`Optional[X]`** → équivalent à `X | None`
- **Union** (plusieurs types possibles) : `Union[X, Y]` ou `X | Y` (Python ≥ 3.10)
- **Littéraux** : `Literal["BUY", "SELL"]` pour valeurs exactes
- **Alias de type** : `Price = float`

---
### **2️⃣ Collections et conteneurs typés**

- `List[X]`, `Dict[K, V]`, `Set[X]`, `Tuple[X, Y]`, `Sequence[X]`, `Mapping[K, V]`
- `list`, `dict`, `tuple` (forme _native_ depuis Python 3.9, ex : `list[int]`)
- **Conteneurs immuables** : `FrozenSet[X]`
- **Iterables** : `Iterable[X]`, `Iterator[X]`, `Generator[Y, S, R]`

---
### **3️⃣ Généricité et type variables**

- **`TypeVar`** → lie types d’entrée/sortie
- **`Generic[T]`** → classes ou interfaces paramétrées
- **`TypeVar(..., bound=Base)`** → restreindre à une classe ou sous-classes
- **Contraintes** : `TypeVar(..., str, bytes)`
- **Variance** :
    - `covariant=True` → sous-types acceptés en sortie
    - `contravariant=True` → sous-types acceptés en entrée

---
### **4️⃣ Structures d’interface**

- **`Protocol`** → duck typing typé
- **`TypedDict`** → dictionnaires avec clés typées
- **`NamedTuple`** → tuples nommés avec champs typés
- **`dataclass`** avec annotations
- **`ABC`** (classes abstraites) + typage strict sur les méthodes
- **`NewType`** → crée un type distinct basé sur un autre (`UserId = NewType("UserId", int)`)

---
### **5️⃣ Types spéciaux pour fonctions**

- `Callable[[Arg1Type, Arg2Type], ReturnType]`
- `ParamSpec` → pour propager la signature complète d’une fonction
- `Concatenate` → pour préfixer/ajouter des arguments à une fonction dans un décorateur
- `Any` → type passe-partout (à limiter)
- `Never` → fonction qui ne retourne jamais (`raise` uniquement)
- `NoReturn` → idem mais pour versions antérieures
- `Self` (Python 3.11+) → type qui représente l’instance de la classe courante

---
### **6️⃣ Méta-typage et introspection**

- `Type[X]` → type d’un objet ou sous-classe de X
- `LiteralString` (Python 3.11+) → chaîne connue à la compilation
- `Annotated[T, ...]` → attacher des métadonnées à un type
- `Final` → indique qu’une variable ou méthode ne doit pas être redéfinie
- `ClassVar[T]` → attribut de classe (pas d’instance)
- `Final` sur classes ou méthodes → empêche l’héritage ou la redéfinition
- `override` (Python 3.12) → marque qu’une méthode redéfinit une méthode parent

---
### **7️⃣ Types pour async**

- `Awaitable[T]`
- `AsyncIterable[T]`
- `AsyncIterator[T]`
- `Coroutine[Y, S, R]`

---
### **8️⃣ Gestion des types externes**

- **Stub files** (`.pyi`)
- **`typing_extensions`** pour compatibilité ascendante
- **`reveal_type()`** dans `mypy` pour introspection
- **`# type: ignore`** et variantes ciblées (`# type: ignore[arg-type]`)
- **Stubs tiers** : `types-requests`, `types-setuptools`, etc.
