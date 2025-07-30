## Pydantic — C’est quoi ?

**Pydantic** est une **librairie Python** qui permet de créer des **objets typés et validés automatiquement à partir de dictionnaires**.  
C’est comme un **dataclass sous stéroïdes**, avec validation automatique, parsing, documentation, introspection, etc.

## À quoi ça sert concrètement ?

Tu veux transformer un dictionnaire brut (ex. JSON, config utilisateur) en objet Python typé, **en t’assurant que les types sont bons** ?

Exemple sans Pydantic :

``` python
config = CsvConfig(delimiter=user_dict["delimiter"], path=user_dict["path"])
```

Avec **Pydantic**, tu peux faire :

``` python
config = CsvConfig(**user_dict)  # et Pydantic valide les types pour toi
```

## 🔧 Exemple concret

``` python
from pydantic import BaseModel

class CsvConfig(BaseModel):
    delimiter: str
    path: str

# Un input brut
user_input = {"delimiter": ";", "path": "output.csv"}

# Automatiquement validé et converti
cfg = CsvConfig(**user_input)
```

Et si l’utilisateur met un int ?

``` python
user_input = {"delimiter": 1, "path": "output.csv"}
CsvConfig(**user_input)  # ❌ ValueError
```

## 🛠️ Bonus : fonctionnalités puissantes

|Fonction|Description|
|---|---|
|✅ **Validation automatique**|types simples (str, int, float…) et complexes (URL, email, etc.)|
|🔄 **Parsing intelligent**|convertit `"123"` en `int` si possible|
|📦 **Sérialisation JSON**|`.dict()`, `.json()`|
|🧪 **Testabilité**|parfaite pour écrire des tests sur des configs|
|🧭 **Introspection**|`.schema()` → génère le schéma JSON|
|🧱 **Support de l’héritage**|`class ExcelConfig(CsvConfig)`…|
|🧬 **Union, Option, Nested**|`Optional[...]`, `List[...]`, `Union[...]`…|
## 🚀 Conclusion

**Pydantic** est le **standard pro** quand :

- tu manipules des `dict`
- tu veux de la validation stricte mais flexible
- tu veux un typage fort tout en gardant Python fluide