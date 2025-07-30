## Pourquoi utiliser un _context_ ?

Un `Context` sert à **regrouper des infos liées entre elles**, **transmises à plusieurs classes**, **utiles dans un scénario donné**.  
C’est un **conteneur sémantique** et structurant.

> ➤ Tu ne crées un contexte **que s’il y a un regroupement logique de données utilisées ensemble** dans plusieurs endroits.

### 🔁 1. **Dans quel cas passer un _contexte_ ?**

Tu passes un objet `context` quand :

- ✅ Tu veux **regrouper plusieurs paramètres liés** (ex : `asset_type`, `exchange`, etc.).
- ✅ Tu veux **évoluer facilement** (ajouter une info sans casser les appels).
- ✅ Tu veux **réduire le couplage** entre les composants (ex : éviter de passer 5 paramètres à chaque fois).  
- ✅ Tu veux **modulariser** les responsabilités (ex : une stratégie de nommage, un log exporter, etc.).
   
🎯 `ExportContext` est une “photographie de l’instant d’export”. C’est un bon usage du contexte ici.