## `AppSettings` : dépendance transversale de configuration

`AppSettings` contient généralement :

- des **valeurs stables** de configuration globale,
- des **chemins de base**, des flags, des options,
- des éléments utilisés par plusieurs services (export, log, stratégie, etc.).

### 🗂️ 2. **`AppSettings` : tout dans une classe ou découper ?**

Tu peux faire :
#### A. Une classe globale `AppSettings` (✅ simple au début)

Mais si elle grossit...
#### B. Tu fais :

``` python
@dataclass
class ExportSettings:
    log_dir: Path
    export_format: str

@dataclass
class EnvSettings:
    env: EnvType
    debug: DebugMode

@dataclass
class AppSettings:
    export: ExportSettings
    env: EnvSettings
```


🧠 **Découpe dès que :**
 
 - Des paramètres sont utilisés par des _modules_ différents
 - Tu veux réutiliser une config spécifique (ex : `ExportSettings`) dans un autre sous-système
 - Tu veux injecter seulement ce qu’il faut (ex : éviter d’injecter tout `AppSettings` dans PathBuilder` si seule `ExportSettings` est utile)