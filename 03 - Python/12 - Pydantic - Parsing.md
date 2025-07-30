##  1. **Qu’est-ce que Pydantic ?**

**Pydantic** est une bibliothèque Python permettant de :

- Valider des données automatiquement (`str → int`, etc.)
- Créer des classes de configuration typées (`BaseModel`)
- Générer du JSON Schema, `.dict()`, `.json()` facilement
- Assurer un haut niveau de **fiabilité des inputs** utilisateurs
- Coercer, convertir et tester les données à l’entrée

> ⚠️ Pydantic **v2** (2023) est encore plus rapide : il utilise **Rust** en backend.

---
## 2. Pourquoi l’**encapsuler** dans un `ConfigParser` ?

### 2.1 Pour **éviter le couplage fort**

``` python
# ❌ MAUVAIS : couplé à Pydantic
config = CsvConfig(**raw_dict)
```
### 2.2 Pour **respecter l’interface générique**

``` python
# ✅ PRO : découplé, interchangeable
config = config_parser.parse(raw_dict)
```
### 2.3 Pour **tester/mocker facilement**

``` python
class FakeParser:
    def parse(self, d): return CsvConfig(delimiter=";", path="test.csv")
```
### 2.4 Pour **changer de moteur de parsing** plus tard

Tu pourras passer de `Pydantic` à `dacite`, `YAML`, `Attrs`, etc.  
**Sans changer le code métier.**

---
## 🛠️ 3. Implémentation du parser typé

### 3.1 Interface générique

 ``` python
from abc import ABC, abstractmethod
from typing import TypeVar, Generic

C = TypeVar("C")

class IConfigParser(ABC, Generic[C]):
    @abstractmethod
    def parse(self, raw: dict) -> C:
        ...
```
### 3.2 Wrapper Pydantic

``` python
from typing import Type

class PydanticConfigParser(IConfigParser[C]):
    def __init__(self, config_class: Type[C]):
        self.config_class = config_class

    def parse(self, raw: dict) -> C:
        return self.config_class(**raw)
```

---
## 4. Exemple complet

### 4.1 Modèle

``` python
from pydantic import BaseModel

class CsvConfig(BaseModel):
    delimiter: str
    path: str
```

#### 4.1.1 🧬 `class CsvConfig(BaseModel)` → Que fait `BaseModel` ?

Quand tu écris :

``` python
class CsvConfig(BaseModel):
    delimiter: str
    path: str
```

Tu dis en réalité :

> « Je définis un modèle **typé, validé, converti et contrôlé** par Pydantic. »


### 4.2 Builder utilisant le parser

``` python
class CsvTargetBuilder:
    def __init__(self):
        self.config_parser = PydanticConfigParser(CsvConfig)

    def build(self, raw: dict):
        config = self.config_parser.parse(raw)
        return CsvExportTarget(config)
```
































