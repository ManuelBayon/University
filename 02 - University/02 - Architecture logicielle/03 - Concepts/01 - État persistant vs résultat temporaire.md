### 1. Pourquoi cette question est cruciale ?

Quand tu conçois une classe, tu te demandes souvent :

> “Dois-je juste calculer une valeur, ou la **mémoriser** dans un attribut ?”

Autrement dit :  
**Dois-je garder une _trace_ d’un élément dans le temps ou juste le renvoyer ?**

---
### 2. Définition clé

| Terme                               | Définition                                                                                                    |
| ----------------------------------- | ------------------------------------------------------------------------------------------------------------- |
| **Attribut d’état (stateful)**      | Information **persistée** dans l’objet, qui **évolue dans le temps**, souvent utilisée par plusieurs méthodes |
| **Résultat temporaire (stateless)** | Valeur calculée **à la volée**, utilisée immédiatement puis oubliée                                           |

---
### 3. Règle d’or à retenir

> ❝ Si une donnée reflète **l’évolution du système**,  
> et qu’elle peut être **utilisée plus tard** dans le code ou par un autre composant,  
> alors elle doit être **stockée comme attribut de l’objet**. ❞
### 4. Bonnes pratiques

- **Met à jour l’état** via une méthode claire (ex: `classify()`, `update_position()`)
- **Expose les états si besoin** via des getters ou du logging
- **Ne surstocke pas** non plus : tout ce qui peut être recalculé à la volée **et n’a pas d’impact dans le temps** → pas besoin d’attribut
