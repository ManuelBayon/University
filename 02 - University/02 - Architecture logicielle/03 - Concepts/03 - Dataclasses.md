##### Types de champs - "identité" et "rôle opérationnel"

###### Exemple :

📌 **L'identité**
- `id : int` → identifiant unique
- `timestamp : datetime` → ordre temporel exact pour les backtests ou l’audit
- `is_active : bool` → suivi du statut sans suppression (auditabilité, reprocessing)

⚖️ **Le rôle opérationnel**

- `type : FIFOOperationType` → ouverture ou fermeture
- `side : FIFOSide` → long ou short
- `quantity : float` → taille de la position
- `price : float` → prix d'entrée ou de sortie