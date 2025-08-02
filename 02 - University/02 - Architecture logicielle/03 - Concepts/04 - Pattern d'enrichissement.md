
> Une instance partielle est enrichie **de façon déclarative**, via une méthode qui retourne une nouvelle version complétée, sans la muter.

### Signature typique :

``` python
complete_obj = partial_obj.with_defaults_from(source_obj)
```

## Pourquoi ce pattern est **très utilisé** (et pro)

|Bénéfice|Détail|
|---|---|
|**Immuabilité**|Pas de mutation d’état → évite les effets de bord|
|**Lisibilité**|On sait que `complete_obj` est prêt à l’emploi|
|**Testabilité**|Tu peux tester `with_defaults_from(...)` indépendamment|
|**Prévisibilité**|Aucun impact sur l’`objet d’origine`|
|**Chaining friendly**|Composable avec d’autres appels (`obj.with_x().with_y()`)|
## 📌 Et en Python dataclass : c’est un _standard implicite_

Grâce à `dataclasses.replace()` (ou `__copy__`/`__deepcopy__`), c’est **naturel, lisible, robuste**.
