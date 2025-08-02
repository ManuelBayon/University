Quand tu fais :

`T = TypeVar('T')`

Tu dis :

> "Je vais utiliser un **type générique**, que je vais appeler `T`, et ce `T` sera **cohérent partout** dans cette classe ou fonction."

Donc si tu écris :
``` python
class IDataProvider(Generic[T]):
    def add_data(self, entry: T) -> None:
        ...
    def get_data(self) -> list[T]:
        ...
```
Alors :

- Si une classe concrète choisit `T = LogEntry`, **toutes** les méthodes utiliseront `LogEntry`.
- Tu ne pourras pas avoir un `add_data(LogEntry)` puis un `get_data() -> list[float]` sans violer le typage.

---
### ✅ Et c’est mieux que `Any`

Parce que `Any`, lui, dit :

> "Je m’en fous de ce que c’est, ça peut changer, tout est permis."

Alors que `T`, lui, dit :

> "Je ne sais pas encore ce que c’est, **mais ce sera le même type partout**."

---

### ➕ Bonus

Tu peux même faire :

`U = TypeVar("U", bound=BaseClass)`

pour dire :

> "U peut être n’importe quel sous-type de `BaseClass`."  
> Donc tu peux avoir des **contraintes** de type.