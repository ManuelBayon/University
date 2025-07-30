
Une **dispatch table** est un **dictionnaire (ou une table de hachage)** qui mappe une **entrée** (souvent une paire `(état, événement)`) à une **fonction à exécuter**.

👉 Elle remplace les `if` ou `match` imbriqués par une structure claire, efficace et extensible.

### Principe de base

Imagine une machine à états. Plutôt que faire :
``` python
if state == "FLAT":
    if event == "GoLong":
        return "OPEN_LONG"
```

Tu fais :
``` python
dispatch_table = {
    ("FLAT", "GoLong"): handle_open_long,
    ("FLAT", "GoShort"): handle_open_short,
    ...
}
```

Et ensuite :
``` python
transition = dispatch_table[(state, event)]()
```

### Comment ça marche en détail
#### 1. **Les clés** : un tuple `(état, événement)`

C’est ce que tu veux détecter.
``` python
("LONG", "GoFlat")  # clé qui représente une situation précise
```

#### 2. **Les valeurs** : des fonctions (ou lambdas)

Ce sont des **fonctions de traitement** qui renvoient la transition, ou exécutent une action.
``` python
def handle_close_long():
    return "CLOSE_LONG"
```

### Exemple simple

``` python
def handle_open_long(): return "OPEN_LONG"
def handle_open_short(): return "OPEN_SHORT"

dispatch_table = {
    ("FLAT", "GoLong"): handle_open_long,
    ("FLAT", "GoShort"): handle_open_short,
}

state = "FLAT"
event = "GoLong"

transition = dispatch_table[(state, event)]()  # -> "OPEN_LONG"
```

### Avec des arguments

Les fonctions peuvent aussi prendre des arguments comme `current_position`, `target_position` :
``` python
def handle_long_to_short(curr, target):
    return "REVERSAL_TO_SHORT"

dispatch_table = {
    ("LONG", "GoShort"): handle_long_to_short,
}

state = "LONG"
event = "GoShort"
result = dispatch_table[(state, event)](current_position, target_position)
```

### Avantages d’une dispatch table

| Avantage            | Explication                                             |
| ------------------- | ------------------------------------------------------- |
| 🔍 **Lisible**      | On voit toutes les transitions dans une seule structure |
| 🧪 **Testable**     | Chaque fonction peut être testée indépendamment         |
| 🔧 **Modulaire**    | Tu peux ajouter/enlever facilement des cas              |
| ⚡ **Performant**    | Lookup en temps constant `O(1)` grâce au dict           |
| 📄 **Documentable** | On peut générer une doc à partir de la table            |

---
## Étapes pro pour créer une `dispatch table`


### **1. Clarifie les dimensions de la logique**

**Question :** Quels sont les **inputs discriminants** qui influencent le comportement ?

 Exemples typiques : 
- `state`, `event` → logique d’automate
- `instrument_type`, `market_regime`, `signal_type` → stratégie
- `user_role`, `action` → sécurité / permissions

Dans ton cas :
``` python
clé = (CurrentState, Event)
```

### 2. Définis les valeurs de la table : des fonctions claires

Chaque valeur est :

- une **fonction métier bien nommée**
- idéalement **pure** (même entrée → même sortie)
- avec une **signature cohérente** (même nombre d’arguments)

💡 Pro tip :
Prends le temps de **donner un nom** à chaque comportement métier.  
→ Ça rend la table _lisible_, testable, extensible.

#### Schéma de nommage pro

Voici une formule éprouvée :

``` text
handle_<action>_from_<state>
```

ou

``` text
handle_<event>_from_<state>
```

### 3. Écris la dispatch table à plat

``` python
dispatch_table = {
    (CurrentState.LONG, Event.GoFlat): close_long,
    ...
}
```

### 4. Écrire chaque fonction individuellement

Les `_, __, ___` sont des **placeholders** pour arguments ignorés.

Parce que dans ta fonction `classify`, tu appelles **chaque fonction de la table** comme ceci :
``` python
func(current_state, event, current_position, target_position)
```

Donc **toutes les fonctions** référencées dans la `dispatch_table` doivent **accepter ces 4 arguments**, même si elles n’en utilisent que 2.

### 5. Écrire la fonction classify
### . Teste chaque fonction individuellement

### . Ajoute une gestion d’erreur propre (KeyError)

Dans le monde pro, tu veux :

``` python
try:
    func = dispatch_table[(state, event)]
except KeyError:
    raise ValueError(f"No transition defined for ({state}, {event})")
```

