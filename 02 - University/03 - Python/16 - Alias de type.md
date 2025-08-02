## 🔎 Le concept

Un **alias de type** (ou _type alias_) en Python, c’est simplement un raccourci pour un type complexe, souvent générique.

Par exemple :

``` python
from typing import TypeAlias

UserId: TypeAlias = int
```

Tu peux maintenant écrire `UserId` dans tes annotations au lieu de `int`, ce qui rend ton code **plus lisible et plus explicite**.

## ✅ Dans ton cas : pourquoi en faire un pour `ExportComponentsDescriptor` ?

Parce que le type :

``` python
ExportComponentsDescriptor[C, T, R]
```

est **très verbeux** dans des endroits comme un registre de plugins :

``` python
_registry: dict[ExportFormat, ExportComponentsDescriptor[C, T, R]]
```

→ Tu peux donc créer un **alias explicite** :

### 🔁 Exemple concret :

``` python
from typing import TypeAlias

# Dans descriptors.py (ou un fichier types.py)
ExportDescriptor: TypeAlias = ExportComponentsDescriptor[C, T, R]
```

Et tu l’utilises ainsi dans ton code :

``` python
_registry: dict[ExportFormat, ExportDescriptor]
```

#### **Attention**
Ne pas redonner le **même nom** à l’alias que la classe originale. 
👉 Ça **écrase** la classe dans le namespace : `ExportComponentsDescriptor` ne désigne plus la classe, mais l’alias.