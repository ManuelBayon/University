## Définition claire : **Plugin injecté**

> Un **plugin injecté**, c’est un **composant externe**, souvent **défini par un contrat (interface / base class)**, et **passé dynamiquement** à ton système via une config, un registre, ou une factory.

### 🎯 Pourquoi on les appelle "plugins" ?

Parce qu’ils sont :

- **Remplaçables** : tu peux plugger un `CSVFormatter` à la place d’un `ExcelFormatter`
- **Découplés** : ils ne connaissent pas le moteur principal
- **Injectés** : tu ne fais pas `formatter = ExcelFormatter()` en dur, tu fais :

``` python
formatter_cls = descriptor.formatter_cls  # <- injecté depuis un registre
formatter = formatter_cls()
```

Donc tu **“pluggues” dynamiquement un composant dans une architecture générique.**

---
## 🤯 Pourquoi c’est critique de valider un plugin injecté ?

Parce que **ton code ne sait pas à l’avance** :

- Ce que c’est
- Ce qu’il fait
- S’il respecte bien le contrat que tu attends

Donc sans validation, tu risques :

- Des erreurs silencieuses (`object has no attribute 'format'`)
- Des comportements bizarres (`None` en sortie au lieu d’un DataFrame)
- Des effets secondaires (`builder.build()` qui ne construit rien...)

