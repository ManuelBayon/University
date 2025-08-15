### **1️⃣ Validation “manuelle”**

- **`assert`** → vérifie une condition et lève `AssertionError` si faux
- **Vérifications explicites** dans le code :
``` python
if not isinstance(price, float):
    raise TypeError("price must be a float")
if price <= 0:
    raise ValueError("price must be positive")
```

---
### **2️⃣ `dataclasses` + `__post_init__`**

- Permet d’initialiser un objet et de valider ses champs après construction
``` python
from dataclasses import dataclass

@dataclass
class Order:
    symbol: str
    quantity: int

    def __post_init__(self):
        if self.quantity <= 0:
            raise ValueError("Quantity must be > 0")
```

---
### **3️⃣ `pydantic` (top-tier dans la finance)**

- Validation + parsing **automatique** sur base des annotations
- Cast automatique des types quand possible
- Validation sur attributs imbriqués
- Erreurs détaillées
``` python
from pydantic import BaseModel, Field

class Order(BaseModel):
    symbol: str
    quantity: int = Field(gt=0)

o = Order(symbol="AAPL", quantity="10")  # OK → cast str→int
o = Order(symbol="AAPL", quantity=-5)   # ❌ ValidationError
```

---
### **4️⃣ `attrs`**

- Similaire à `dataclasses` mais plus flexible
- Hooks de validation
``` python
import attr

@attr.s
class Order:
    symbol = attr.ib(type=str)
    quantity = attr.ib(type=int, validator=attr.validators.gt(0))
```

---
### **5️⃣ Protocols de validation personnalisés**

- Utiliser des **`Protocol`** ou ABC pour forcer les implémentations à avoir une méthode de validation
``` python
from typing import Protocol

class Validatable(Protocol):
    def validate(self) -> None: ...
```

---
### **6️⃣ Décorateurs de validation**

- Ajouter un wrapper autour de méthodes pour vérifier les inputs/outputs
``` python
def ensure_positive(fn):
    def wrapper(*args, **kwargs):
        result = fn(*args, **kwargs)
        if result <= 0:
            raise ValueError("Result must be positive")
        return result
    return wrapper
```

---
### **7️⃣ Outils de validation de schéma**

- **`jsonschema`** pour JSON structuré
- **`marshmallow`** pour sérialisation/désérialisation + validation

---

### **8️⃣ Intégration validation runtime + typage statique**

- Annoter avec `pydantic` ou `attrs` + valider au runtime
- Les annotations servent à **mypy** pour la partie statique
- Les règles runtime (Field constraints, validators) assurent que les données sont réellement correctes en prod

---

💡 **Règle hedge fund** : 
Toujours valider aux deux niveaux :
- **Statique** → éviter 90% des incohérences pendant le dev (`mypy`, IDE, CI/CD)
- **Runtime** → protéger en prod contre les données extérieures, corruptions, ou bugs internes