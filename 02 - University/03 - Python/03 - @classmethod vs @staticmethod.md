### 🔧 `@classmethod`

- **Première différence** : la méthode reçoit **la classe (`cls`)** en premier argument, pas l'instance (`self`).
- Elle peut accéder et modifier des attributs ou méthodes **de classe**.
- Sert souvent de **méthode de fabrique**, pour créer des instances avec une configuration particulière.
#### Exemple :

``` python
class Connexion:
	@classmethod
		def from_localhost(cls):
			return cls(host="127.0.0.1", port=1234)
```

➡️ Ici, `cls` permet d’instancier dynamiquement la classe. Même si on hérite de `Connexion`, `from_localhost()` renverra une instance de la sous-classe.

---

### ⚙️ `@staticmethod`

- Ne reçoit **aucun argument implicite** (ni `self`, ni `cls`).
- Fonction totalement indépendante, juste **logiquement liée** à la classe.
- Sert à regrouper une logique dans la classe sans dépendre de son état.
#### Exemple :

``` python
class MathUtils:
	@staticmethod
		def add(a, b):
			return a + b
```

➡️ Elle pourrait exister en dehors de la classe, mais on la met là pour cohérence.

---
### 🧠 Résumé :

|Décorateur|Premier argument|Accès à l’état de…|Utilisation typique|
|---|---|---|---|
|`@staticmethod`|aucun|Aucun|Fonction utilitaire sans dépendance|
|`@classmethod`|`cls` (la classe)|La **classe**|Méthode de fabrique, surcharge selon classe|
|méthode normale|`self`|L’**instance**|Comportement spécifique à une instance|