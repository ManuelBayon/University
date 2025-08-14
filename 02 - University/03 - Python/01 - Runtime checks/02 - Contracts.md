## 1. Problème

Dans un système modulaire, les composants (plugins, services, builders, formatters…) sont souvent **injectés dynamiquement**. 

En Python, il n’existe **aucune vérification statique stricte** (comme en Java ou Rust) pour garantir que ces objets respectent bien le contrat attendu.

⚠️ Cela crée un risque d’erreur **tardive et silencieuse** :  
un `.build()` ou `.export()` manquant, un `TypeError` en prod, un format de sortie incohérent, etc.

---
## 2. Solution

Créer un module `validators.py` dans le répertoire `.\core`, qui fournit des fonctions de vérification explicites, comme :

Ce module agit comme un **substitut dynamique d’interface**, garantissant que :

- Chaque **plugin injecté** respecte bien le contrat implicite attendu
- Chaque **composant métier** (builder, formatter, exporter…) implémente bien ses méthodes clés

**Attention :** Penser à créer un fichier test pour chaque vérification runtime avec un exemple fonctionnel et dysfonctionnel.

---
## 3. Patterns

| Principe                          | Description                                                                         |
| --------------------------------- | ----------------------------------------------------------------------------------- |
| **Centraliser les vérifications** | Ne pas faire de `hasattr` dispersés, tout regrouper dans `validators.py`            |
| **Fail fast**                     | Valider dès l’injection ou au tout début d’un appel public (`build()`, `export()`)  |
| **Être explicite**                | Nommer clairement les attentes : `.format`, `.to_dict`, `.save`, etc.               |
| **Modulariser les contrats**      | Regrouper les attentes par rôle (Builder, Exporter, Formatter…) pour les réutiliser |

---
## 4. Exemple

## Point d'entrée n°1 : PluginRegistry.register_components()

C’est l’endroit où un plugin est inscrit. Il doit être **bien formé dès le départ**.

**À faire :** Créer une fonction dans `validators.py`  dans le répertoire core:

``` python
REQUIRED_DESCRIPTOR_ATTRS = [  
    "target_config_cls",  
    "formatter_cls",  
    "target_builder_cls",  
    "exporter_cls",  
    "service_cls"  
]
def validate_descriptor_contract(  
        descriptor: object,  
        name: str = "Descriptor"  
) -> None:  
    for attr in REQUIRED_DESCRIPTOR_ATTRS:  
        if not hasattr(descriptor, attr):  
            raise TypeError(f"{name} is missing attribute: '{attr}'")  
  
def validate_enum_instance_of(  
        value: object,  
        enum_cls: type[Enum],  
        name: str = "value"  
) -> None:  
    if not isinstance(value, enum_cls):  
        raise TypeError(f"Expected '{name}' to be {enum_cls.__name__}, got {type(value).__name__}")
```

**Et l’ajouter ici :**
``` python
@classmethod  
def register_components(  
        cls,  
        format: ExportFormat,  
        descriptor: ExportDescriptorType  
) -> None:  
    validate_descriptor_contract(descriptor)  
    validate_enum_instance_of(format, ExportFormat, "format")  
    cls._registry[format] = descriptor
```

**Créer un fichier de test pour valider les fonctions de validation avec un exemple fonctionnel et un exemple dysfonctionnel :**




