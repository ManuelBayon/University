### **1. Règles métier explicites**

- Définitions claires des **contraintes logiques** propres à ton domaine
- Ex. : un ordre de bourse ne peut pas être exécuté avant son heure d’ouverture
- Outils : fonctions dédiées, validators Pydantic personnalisés, méthodes `.validate()`

---
### **2. Invariants**

- Conditions qui **doivent rester vraies** tout au long de la vie de l’objet
- Ex. : le solde d’un compte ne peut pas être négatif (sauf découvert autorisé)
- Outils : assertions, décorateurs de vérification, tests d’intégrité après chaque mutation

---
### **3. Contraintes inter-objets**

- Relations entre plusieurs entités du système
- Ex. : un instrument financier doit appartenir à un marché qui accepte son type d’actif
- Outils : services de validation centralisés, agrégats (pattern DDD)

---
### **4. Règles temporelles**

- Cohérence dans le temps (passé, présent, futur)
- Ex. : une transaction ne peut pas être datée après la clôture de marché
- Outils : comparaisons de timestamps, calendriers métier, librairies comme `pandas` ou `dateutil`

---
### **5. Scénarios exceptionnels**

- Cas où les règles changent selon le contexte
- Ex. : suspension de cotation → certaines validations sont temporairement levées
- Outils : “policy objects” ou règles paramétrées

---
### **6. Tests de cohérence automatisés**

- Scripts ou jobs réguliers qui passent en revue les données et lèvent des alertes si incohérence
- Outils : jobs ETL de validation, dashboards de monitoring