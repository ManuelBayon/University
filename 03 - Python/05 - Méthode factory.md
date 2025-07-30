## Objectif d'une factory de service

Créer un point d’entrée unique qui :
1. **Choisit la bonne implémentation** d’un service (`ExcelLogExportService`, `GoogleSheetsExportService`, etc.),
2. **Assemble toutes les dépendances internes** (exporter, builder, context, logs),
3. **Retourne une instance prête à l’emploi**, via un type abstrait (`IExportService`).

Elle n’appartient ni à l’interface `IExportService`, ni à l’implémentation concrète.  
Elle vit **à part**, souvent dans un module nommé `factories`, `builders`, `services`, ou `application`.

---
## Comportement attendu d’une factory :

- **Stateless** 🧽 : elle ne garde pas de données.
- **Pure** : même inputs → même output (objet créé).
- **Statique** : généralement, la méthode est `@staticmethod` ou `@classmethod`.

Exemple Python classique :
``` python
class ExportServiceFactory:
	@staticmethod
	def create(...):
		# instanciation logique ici         
		return ExportService(...)
```

---

## ❌ Quand **éviter** de stocker des choses :

Tu ne veux **pas** que la factory :

- garde en mémoire des paramètres (`self._context`, etc.)
- soit mutable
- doive être instanciée (`factory = ExportFactory()`)

---

## 🧠 Exceptions rares :

Parfois, on a des **"factory avec état"**, mais uniquement dans des cas très particuliers :

- **Abstract Factory Pattern** avec polymorphisme complexe
- Factory paramétrée pour générer des produits avec un même contexte
- Factory servant de **container DI** (ex : gestion de dépendances configurables)

Mais c’est rare, avancé, et **hors de ton cas ici.**