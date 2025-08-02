Une **logique de fallback** (ou de repli) est une **valeur ou un comportement alternatif**  
qui est utilisé **lorsqu’une information principale est absente, invalide ou non spécifiée.**

## Pourquoi c’est important

- Tu veux que ton système **reste robuste même si certaines infos sont manquantes**
- Tu veux éviter les `None`, les erreurs silencieuses, ou les comportements incohérents
- Tu veux que ton système se comporte **de manière prévisible et contrôlée**

### Exemples concrets dans ton système

#### 1. `what_to_show` non précisé par l’utilisateur

``` python
request = IBKRRequestSettings(     duration="30 D",     bar_size="1 day",     what_to_show=None )  request = request.with_defaults_from(instrument) # → fallback vers instrument.default_what_to_show()
```

#### 2. Fallback de type `Optional[...]` dans une dataclass

```python
exchange: Optional[str] = None
```

Puis dans ton `to_contract()` :

``` python
exchange = self.exchange or "SMART"
```

✅ Si `exchange` n’est pas défini, tu tombes sur `"SMART"`.

### 3. Fallback comportemental (dans le code)

``` python
def fetch_data(...):     
	if not self.is_connected():         
		self.connect()
```

✅ Si la connexion n’est pas active, tu la rétablis **en fallback**, avant de poursuivre.

## 🧠 Règle pro : "Fail gracefully"

Un bon système ne se plante pas brutalement pour un détail manquant.  
Il **tente de compléter, remplacer, ou réagir intelligemment.**

Mais il **log** toujours quand il utilise un fallback.

## 💡 Modèles courants de fallback

|Pattern|Exemple|
|---|---|
|✅ **Valeur par défaut**|`self.value or "default"`|
|✅ **Méthode déléguée**|`.with_defaults_from(...)`|
|✅ **Hiérarchie de résolution**|config utilisateur > config projet > config système|
|✅ **Retry ou reconnect**|Si l’appel échoue, tenter un plan B|
|✅ **Fallback polymorphe**|`instrument.default_what_to_show()` selon le type|
## 🎓 Bonus : distinction pro

|Terme|Définit|
|---|---|
|**Default**|Valeur utilisée par convention si rien n’est fourni|
|**Fallback**|Stratégie de repli dynamique si l’info attendue est absente, invalide ou cassée|

Le fallback est souvent **contextuel**, là où un `default` est plus **statique**.

## ✅ Résumé

|Concept|À retenir|
|---|---|
|Logique de fallback|Comportement ou valeur alternative utilisée quand l’info principale est absente|
|But|Robustesse, continuité, prévisibilité|
|Formes|valeur par défaut, méthode de délégation, retry, substitution|
|Dans ton système|`what_to_show`, `exchange`, `use_RTH`, etc. sont des candidats typiques|
|Bonne pratique|Loguer quand un fallback est activé (en prod)|