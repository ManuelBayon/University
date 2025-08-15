## Posture runtime des hedge funds d’élite

> “**Tu valides dynamiquement tout ce que tu ne peux pas garantir statiquement.**  
> Et **tu le fais tôt, bruyamment, et traçablement**.”

Un lead dev dans un hedge fund d’élite validera systématiquement :
- Ce que le typage statique **ne peut pas garantir**
- Toute donnée ou objet **venant de l’extérieur du cœur système** (plugins, configs, data)
- Toute sortie d’un composant **injecté** (formatter, builder, exporter)

🔐 Objectifs :

- Éviter les “bugs silencieux”
- Rendre chaque étape du pipeline **safe, traçable, testable**
- Crasher proprement si un contrat est violé
- Permettre le **debug post-mortem** via logs et IDs d’exécution

---
## 🔍 POUR SAVOIR _QUOI TESTER AU RUNTIME_

### 1. **“Est-ce que ça vient de _l’extérieur_ ?”**

> ⚠️ Si oui → toujours valider.

- Plugin injecté (`formatter`, `builder`, `strategy`, `model`)
- Input utilisateur (CLI, config, API)
- Fichier ou chemin (`pathlib.Path`)
- Données (`list`, `DataFrame`, JSON…)

✅ **Pourquoi ?** Ce n’est pas toi qui les maîtrises.

### **2. “Est-ce que ça passe une _frontière invisible_ ?”**

> Exemple : entre 2 modules, 2 couches, 2 équipes.

- Tu passes du `format()` au `builder.build()`
- Tu envoies des données dans un service externe
- Tu passes d’une couche typée à une couche dynamique

✅ **Pourquoi ?** Le contrat peut être cassé sans bruit.

### **3. “Est-ce que je pourrais avoir un bug silencieux ici ?”**

> ex : un `.format()` renvoie une mauvaise structure, mais valide au sens Python

- `list[dict]` attendu mais `list[str]` donné → pas de crash immédiat
- méthode `.export()` manquante → pas d’erreur avant l’appel
- valeur `None` qui passe dans un champ `str`

✅ **Pourquoi ?** Tu veux éviter les erreurs _tardives et obscures_.

### **4. “Est-ce que c’est critique pour le système ?”**

> Toute donnée utilisée pour :

- Générer de l’argent (ex : une stratégie, un signal)
- Communiquer avec le monde externe (ex : export, appel API)
- Modifier un état central (base, file, mémoire, log)

✅ **Pourquoi ?** Un bug ici coûte de l’argent ou de la crédibilité.

## 🔐 Typologie des validations runtime **obligatoires en hedge fund top tier**

|Type de validation|Exemple concret|
|---|---|
|`assert_type()`|`assert_type(config, ExcelTargetConfig)`|
|`assert_iterable_of()`|`assert_iterable_of(data, TradeEvent)`|
|`assert hasattr()`|Vérifie que le plugin a `.build()` ou `.export()`|
|`check_path()`|Vérifie que le dossier existe, que l’extension est valide|
|`check_dataframe_shape`|Vérifie que le format de sortie respecte un schéma ou des colonnes attendues|
|`check_config_fields()`|Chaque champ critique (ex : `path`, `id`, `mode`) est validé|
|`check_not_none()`|Aucune étape du pipeline ne retourne `None` sans raison|
|`assert isinstance(x, Interface)`|Surtout si `x` est injecté dynamiquement|

---
