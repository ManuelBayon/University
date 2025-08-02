## Description
### 1. Le dictionnaire (`dict`) : la _table d’association_
#### Rôle : contenir une correspondance entre une _clé_ et une _valeur_

Dans ton cas :
``` python
dispatch_table: dict[Tuple[CurrentState, Event], Callable]
```

- **Clé** = un `tuple` représentant `(state, event)`
- **Valeur** = une fonction à appeler
#### ✅ Avantages du dictionnaire
- Lookup ultra rapide : `O(1)`
- Lisibilité : tu vois toutes les transitions en un seul endroit
- Extensible dynamiquement
- Tu peux le remplir à partir d’un fichier `.yaml`, `.json`, etc.
#### ❌ Limites
- Pas de logique directement dans le dict (c’est juste une map)

### 2. Le tuple (`tuple`) : la _clé composée_
#### Rôle : représenter une combinaison unique de `(état, événement)`

Tu veux capturer :  
→ “quand je suis dans cet `état` et que je reçois cet `événement`, je fais quoi ?”

Exemple :
``` python
(CurrentState.LONG, Event.GoShort)
```

C’est une **clé immuable**, parfaite pour les dictionnaires.

#### ✅ Avantages du tuple comme clé
- Immuable → utilisable en `dict`
- Simple et lisible
- Peut contenir autant d’éléments que nécessaire
#### ❌ Limites
- Pas de nommage des champs (contrairement à `dataclass` ou `namedtuple`) 
- Pas fait pour contenir du comportement, juste des données

### 🧠 Analogie simple
🔹 Le `dict` est le **annuaire**.  
🔸 Le `tuple` est **le nom complet** (prénom + nom) qui sert à chercher dans l’annuaire.

## Utilisation dans le monde pro en finance

Dans le monde professionnel — et **encore plus en finance**, en trading, en assurance, ou en systèmes temps réel — la combinaison `tuple` + `dict` est un **pattern ultra courant et éprouvé**, car elle permet :

### 1. **Des lookup rapides et déterministes**

- En finance, tout doit être **rapide, prévisible et sans surprise**.
- Les `dict` Python (hash tables) permettent des accès en temps **constant `O(1)`**.
- Le `tuple` est **hashable et immuable**, donc parfait comme **clé composée** dans un `dict`.

Exemple typique :
``` python
(state, event) → action
(symbole, date) → prix
(courbe de taux, échéance) → valeur interpolée
```

### 2. **Des mappings explicites sans if-else**

Les `dict` à clés tuples sont très utilisés pour :
- Les **dispatch tables** (comme dans ton cas)
- Les **matrices de transition** pour des automates ou moteurs de règles
- Des **grilles de décision** (ex. : `(produit, pays)` → taxe)

### 3. **De la modularité et de la maintenabilité**

Les équipes pros préfèrent :
- Des systèmes **déclaratifs** plutôt que des if-else,
- Des structures **testables, documentables, versionnables**.

Exemple dans un moteur de pricing :
``` python
pricing_logic = {
    ("Option", "Call"): price_call_option,
    ("Option", "Put"): price_put_option,
    ("Swap", "Payer"): price_swap_payer,
    ("Swap", "Receiver"): price_swap_receiver,
}
```

### 4. **Interopérabilité avec YAML / JSON**

Dans les fonds modernes, les règles ou transitions sont parfois **chargées dynamiquement** depuis des fichiers `.yaml`, `.json`, ou une base de données.