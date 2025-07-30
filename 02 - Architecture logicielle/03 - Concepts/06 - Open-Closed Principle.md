### Définition

> **Un module doit être ouvert à l’extension, mais fermé à la modification.**

Cela signifie :

- Je peux **ajouter** de nouveaux comportements sans modifier l’existant.
- Le code **en place reste stable** (non touché).
- Les **nouvelles variantes** sont ajoutées via des classes/fichiers/modules **séparés**.

---
### Pourquoi c’est crucial ?

- Moins de bugs (car on ne touche pas à ce qui marche déjà)
- Système plus modulaire et maintenable
- Scalable (on peut ajouter Excel, JSON, SQL, Kafka sans casser le reste)
- Design pensé pour durer (architecture propre)

---
### Application concrète dans mon projet (Export)

#### ❌ Mauvais exemple (violant OCP)

``` python
def format_logs(logs, format):
    if format == "excel":
        ...
    elif format == "json":
        ...
    elif format == "csv":
        ...
```
➡ Chaque fois que je veux un nouveau format, je modifie cette fonction.  
➡ Risque d'erreur, dette technique, pas extensible.

---
#### ✅ Bon exemple (respectant OCP)
``` python
class ExcelFormatter(ILogFormatter): ...
class JsonFormatter(ILogFormatter): ...
class CsvFormatter(ILogFormatter): ...

FORMATTERS = {
    ExportFormat.EXCEL: ExcelFormatter,
    ExportFormat.JSON: JsonFormatter,
    ExportFormat.CSV: CsvFormatter
}

def resolve(format: ExportFormat) -> ILogFormatter:
    return FORMATTERS[format]()
```

➡ Ajouter un nouveau format = créer une classe et l’enregistrer dans le registry.  
➡ Aucune modification du code central.  
➡ OCP respecté.

---
### Pattern associé

- **Strategy pattern** : chaque stratégie (formatter, target, stratégie de trading…) est encapsulée dans une classe distincte interchangeable.
- **Plugin registry** : registre centralisé pour retrouver la bonne implémentation sans `if`.

---
### Règle d’or à retenir

> Ne code pas pour les cas connus.  
> Code pour permettre aux cas futurs d’être ajoutés **sans toucher** à ceux déjà en place.