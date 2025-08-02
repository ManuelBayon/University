### 1. Définition courte

Un registre centralisé qui permet **d’enregistrer dynamiquement des composants** (plugins, classes, objets) et de les **retrouver plus tard via une clé** (ex. une énumération, un nom ou un type).  
Ce pattern favorise l’**extensibilité** sans modifier le code existant.

### 2. Centraliser le registre (➡️ un seul `ExportPluginRegistry`)

Un bon fond ferait **un registre global unique** (ou au pire, un composant DI/container) qui contient les **mappings entre le format (ou type de cible) et ses composants**.

Par exemple :

``` python
_registry = {
    "excel": {
        "formatter": ExcelLogFormatter,
        "target_builder": ExcelTargetBuilder,
        "log_exporter": ExcelLogExporter,
    },
    ...
}
```

### 3. **Injecter les bons composants à la création du service**

Ton `ExportServiceFactory.create()` devient le point d’orchestration :
- Tu lui passes un format (`"excel"` par ex)
- Il **interroge le registre** pour trouver :
    - le `ILogFormatter`
    - le `ILogExporter`
    - le `ITargetBuilder`
- Puis il instancie proprement ton `ExcelExportService<T>` avec les bons éléments.

👉 C’est **cette fabrique** qui orchestre **en une seule fois**, et qui reste simple à maintenir.

## Pourquoi un seul registre ?

- **Responsabilité claire** : un seul endroit où tu définis les "plugins" par format.
- **Moins de duplication** : pas besoin d’un registre par type.
- **Évolutif** : tu peux ajouter un format `"json"` ou `"sqlite"` d’un seul coup en ajoutant une ligne.
- **Open/Closed Principle** : tu peux étendre sans modifier.


## Bonus : approche ultra propre (inspirée DI containers)

Tu pourrais même aller plus loin :

### Type-safety avec `dataclasses`

``` python
@dataclass
class ExportComponents:
    formatter: Type[ILogFormatter]
    log_exporter: Type[ILogExporter]
    target_builder: Type[ITargetBuilder]
```
Puis dans ton registre :
``` python
_registry = {
    "excel": ExportComponents(...),
    ...
}
```
