## 🧠 Sa démarche : UML → Code → Tests, mais **avec feedback itératif**

Un **lead dev pro** ne code **pas tout d’un coup**, ni **tout tester d’un coup**.

Il fonctionne par **micro-itérations guidées par la clarté** :

---

### 🧭 **Étape 1 — Compréhension & design (UML propre mais pragmatique)**

- Il **dessine un premier schéma UML clair** des composants essentiels (sans trop détailler les dépendances internes).
- Il se demande :
    - Quelle est l’intention ?
    - Qu’est-ce qui doit rester modulaire ?
    - Quels sont les contrats (interfaces) ?
    - Qu’est-ce qui changera avec les formats ?

💬 _“On part d’un besoin explicite et d’une modélisation claire du contrat entre les composants.”_

> 🔧 Il nomme les classes comme des rôles, pas comme des implémentations : `IFormatter`, `ITargetBuilder`, `IExporter`.

---

### 🧱 **Étape 2 — Il code une première version du module (sans tests au début, mais testable)**

- Il écrit un `ExportComponentDescriptor`, un `registry`, une `factory` et un petit module de test manuel.
- Il fait attention à :
    - Ne **pas enterrer les erreurs** (`fail fast`)
    - Structurer le module comme une brique réutilisable
    - Rendre l'instanciation claire et typée

💬 _“Je code comme si quelqu’un d’autre allait devoir lire et réutiliser mon code dans 6 mois.”_

---

### 🧪 **Étape 3 — Il écrit des tests dès que la mécanique tourne (mais pas trop tôt)**

- Pas besoin de 100 tests au début : il écrit **1 test simple par format** pour vérifier que l’usine produit bien les bons objets.
    

> Ex. :

``` python
def test_json_descriptor_instantiates_correctly():
    descriptor = registry.get_components(ExportFormat.JSON)
    components = ExportComponentFactory.instantiate(descriptor, settings, context)

    assert isinstance(components.formatter, IFormatter)
    assert isinstance(components.target_builder, ITargetBuilder)
    assert isinstance(components.exporter, IExporter)
```

Il ne teste **ni les mocks**, ni les détails trop tôt.  
Mais il veut que le système **échoue si le wiring est cassé**.

---

### 🔁 **Étape 4 — Il itère : chaque nouveau format ou bug → nouveau test**

- Il ne sur-teste pas ce qui est stable.
- Il teste **chaque nouveau scénario réel**.
- Il ajoute **des tests de régression** si un bug est corrigé.

💬 _“J’écris des tests pour protéger ce que j’ai compris, pas ce que j’imagine.”_

---

### 🧱 À chaque niveau, il garde une exigence mentale simple :

|Niveau|Sa question-clé|
|---|---|
|UML|_Est-ce que je comprends le rôle de chaque classe ?_|
|Code|_Est-ce que je peux instancier tout sans deviner ?_|
|Tests|_Est-ce que je peux casser une pièce et que ça se voie tout de suite ?_|

---

## 🧠 Sa philosophie profonde

> “La qualité d’un système vient de sa clarté structurelle. Les tests sont un filet de sécurité, pas un substitut à la clarté.”

Il **préfère avoir un système modulaire, limpide et bien pensé**, avec quelques bons tests ciblés…  
… qu’un système illisible couvert de tests superficiels.