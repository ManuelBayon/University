
### 🎯 Problématique formelle :

> **Comment garantir à runtime que le résultat produit par un composant (comme un formatter) est bien du type générique `R` déclaré dans le descriptor – alors que `R` est effacé ?**

---
### 🔍 Description :

Dans un moteur typé génériquement (avec `C`, `T`, `R`), on suppose que les types propagés à travers les interfaces (ex : `IFormatter[T, R]`) garantissent la cohérence des données produites et consommées.

Mais en Python, les types génériques sont **effacés à l’exécution** (`type erasure`) → le type `R` n’existe pas comme entité concrète à runtime.

Cela signifie qu’un formatter peut :

- **déclarer** `R = Json`
- mais **retourner** un `pd.DataFrame`  
    👉 sans qu’aucune erreur ne soit levée automatiquement.

---
### ❗️Conséquence :

Le système peut :

- propager silencieusement des incohérences
- produire des résultats incompatibles (ex : `.format()` retourne un DataFrame alors que le pipeline attend un `dict`)
- **casser tard**, voire **jamais**, si aucun contrôle explicite n’est ajouté

---
### ✅ Solution proposée :

1. **Conserver la trace de `R` au moment de la déclaration** (dans le `ExportComponentsDescriptor`)

2. **Ajouter des vérifications runtime explicites** :
    - Lors du `register_components()`
    - Lors de l’appel `.format(data)`

3. **Définir un contrat clair dans l’architecture** :  
    _"Chaque composant est responsable d’annoncer ce qu’il produit (`output_type`) et de le respecter."_

---
### 🛠 Extrait de pattern à noter :

``` python
result = formatter.format(data)
if not isinstance(result, descriptor.expected_output_type):
    raise TypeError("Formatter output does not match declared R")
```

---
### 🧠 Insight clé :

> En Python, **le typage générique est une aide à la clarté mais pas une garantie de robustesse**.  
> La **vérification runtime** est indispensable dans un moteur modulaire, surtout si on veut un système à la hauteur des standards d’un hedge fund.

