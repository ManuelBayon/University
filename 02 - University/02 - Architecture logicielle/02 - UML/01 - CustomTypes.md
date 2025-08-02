

- Une classe `List` avec un `Paramètre de template` nommé `T`
- Un attribut ou un paramètre typé `List`
- Et que tu lies `T` à un type réel (`ExportTarget`, `string`, etc.)

👉 **Modelio sait automatiquement que tu parles d’un `List<T>` instancié en `List<ExportTarget>`**

Il ne l’écrit pas toujours comme `List<ExportTarget>` dans tous les affichages, mais :

- Il stocke l’info proprement dans le modèle
- Il la réutilise dans la génération de code ou l’export XMI
- Et il peut l’afficher dans les diagrammes si tu actives l’option d’affichage des _template bindings_