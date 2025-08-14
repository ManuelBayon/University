**Les types génériques** sont un outil **puissant, propre et élégant** pour écrire du code réutilisable **tout en gardant un typage fort**. Ils sont **essentiels pour penser comme un architecte**, surtout 
en Python quand tu veux faire du polymorphisme sans perdre la lisibilité.

#### Forme Python :
T = TypeVar("T")
class MonType(Generic[T]): 
	...