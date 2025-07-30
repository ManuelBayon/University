Dans le cadre d'une ComponentFactory j'ai utilisé 2 composants un descriptor qui contient les classes et un component qui contient les classes instancié par la factory. 

Pour typer le descriptor qui contient les classe j'ai utiliser **type\[MonInterface\]**

Exemple:
formatter_cls: type\[IFormatter\]

dans la factory je fais:
descriptor.formatter_cls()