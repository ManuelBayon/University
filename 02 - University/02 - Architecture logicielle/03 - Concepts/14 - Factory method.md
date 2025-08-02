**Définition simplifiée :**

> _Une méthode (généralement statique ou de classe) qui encapsule la création d’un objet au lieu de laisser l’appelant instancier directement via le constructeur._

Cela te permet :

- de centraliser la logique d’initialisation (notamment si elle dépend de `AppSettings`)
- de faire de la **validation** avant instanciation
- de renvoyer potentiellement **des sous-classes**, ou d’autres formats, si nécessaire plus tard

**1. Sans Factory Method**

→ Construction brute avec `__init__` (données simples, sans encapsulation)

``` python
@dataclass
class ExportContext:
    asset_type: AssetType
    exchange: Exchange
    symbol_id: SymbolList
    timestamp: datetime

# Utilisation directe
ctx = ExportContext(
    asset_type=AssetType.FOREX,
    exchange=Exchange.IB,
    symbol_id=SymbolList.EURUSD,
    timestamp=datetime.utcnow()
)
```

###### ✅ Avantages :
- Simple
- Lisible
- Immédiatement instanciable

###### ❌ Limites :
- Toute la **logique métier d’instanciation** est à l’extérieur (répétée, copiée)
- Aucun contrôle sur la cohérence (timestamp ? symbol_id manquant ? settings à utiliser ?)
- Si demain tu veux dériver le contexte ou le valider → c’est foutu

**2. Avec Factory Method (`instantiate()`)**

> → Tu encapsules toute la logique de création dans une méthode statique (ou de classe)

``` python
@dataclass
class ExportContext:
    asset_type: AssetType
    exchange: Exchange
    symbol_id: SymbolList
    timestamp: datetime

    @staticmethod
    def instantiate(settings: AppSettings, context: 'ExportContext') -> 'ExportContext':
        # Exemple de logique : on garde exchange et asset_type du parent
        # mais on change le symbol_id et on prend le timestamp du moment
        return ExportContext(
            asset_type=context.asset_type,
            exchange=context.exchange,
            symbol_id=settings.default_symbol_id,
            timestamp=datetime.utcnow()
        )
```

###### ✅ Avantages :
- Tu **centralises la logique de création** (plus de duplication)
- Tu peux facilement :
    - injecter des `settings`
    - créer un contexte dérivé
    - ajouter de la validation
- Tu **conserves un point d’entrée unique** pour toute création d’objet
- Tu peux retourner une **autre classe** (sous-classe ou proxy, par exemple)

###### ❌ Inconvénients :
- Légèrement plus verbeux
- Moins "Pythonique" au tout début si mal documenté (mais très pro à long terme)

**3. Version alternative avec `@classmethod`**

Pour plus de flexibilité :

``` python
@dataclass
class ExportContext:
    asset_type: AssetType
    exchange: Exchange
    symbol_id: SymbolList
    timestamp: datetime

    @classmethod
    def from_settings(cls, settings: AppSettings, parent: 'ExportContext') -> 'ExportContext':
        return cls(
            asset_type=parent.asset_type,
            exchange=parent.exchange,
            symbol_id=settings.default_symbol_id,
            timestamp=datetime.utcnow()
        )
```

→ Ici, si tu changes le nom de la classe (`ExportContext`) dans le futur, `cls` suit automatiquement 
→ Tu peux aussi l’hériter

## 🧩 En UML ?

- La méthode `instantiate(...)` dans ton diagramme **représente une Factory Method**
- Tu peux l’annoter avec `<<factory>>` ou `<<static>>` si ton outil le permet
- Et documenter dans ta note : _"Factory method centralisant la logique de création de contexte export à partir d’un contexte parent et des settings."_