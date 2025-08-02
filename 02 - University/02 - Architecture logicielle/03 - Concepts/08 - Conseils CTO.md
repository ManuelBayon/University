## 🧠 Détails supplémentaires d’un CTO :

- Chaque fichier `.py` contient **1 à 2 classes maximum**
- Toutes les classes ont une **docstring claire et typée**
- Le nom des modules est **fonctionnel, pas technique** : `excel_formatter` > `formatter_excel`
- Les exceptions sont centralisées (tu peux faire un fichier `exceptions.py` si besoin plus tard)
- Les `Enum` sont typés et versionnables (dans `core/enums.py`)
- Les `LogProvider`, `ExportService`, etc. ont des `__repr__` utiles pour debugging