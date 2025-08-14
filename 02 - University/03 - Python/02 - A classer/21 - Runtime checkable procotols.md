
Ce décorateur est **crucial** pour comprendre comment concilier le **duck typing** (comportement) avec le **type checking statique** (mypy, Pyright) **et dynamique** (`isinstance`).

---
### 🧠 `@runtime_checkable` sur un `Protocol`

Permet de faire :

``` python
if isinstance(obj, MonProtocol):
    ...
```

alors que `Protocol` est une **interface comportementale**, normalement **invisible à l’exécution**.

---
### 🧩 Contexte

#### Les `Protocol` (depuis PEP 544) :

- Un `Protocol` est une **interface implicite** basée sur la **forme** de l’objet.
- Si un objet a les bonnes **méthodes / attributs**, alors il **implémente le `Protocol`** (même sans héritage).
- C’est du **"structural typing"** (≠ "nominal typing").

---

#### Par défaut, les `Protocol` sont **non vérifiables** à l’exécution :

``` python
from typing import Protocol

class Foo(Protocol):
    def bar(self) -> int: ...

obj = ...
isinstance(obj, Foo)  # ❌ TypeError: Protocols cannot be used with isinstance()
```

### ✅ Solution : `@runtime_checkable`

``` python
from typing import Protocol, runtime_checkable

@runtime_checkable
class Foo(Protocol):
    def bar(self) -> int: ...
```

Maintenant :

``` python
if isinstance(obj, Foo):  # ✅ fonctionne
    ...
```

🎉 Tu peux donc tester dynamiquement si un objet implémente ce `Protocol` — **même s’il n’en hérite pas explicitement**.

---
### 📌 Limites de `@runtime_checkable`

- Seules les **méthodes** et **propriétés** accessibles via `hasattr()` sont testées.
- Les annotations ne sont **pas vérifiées à l’exécution**.
- Pas de vérification profonde du type de retour.