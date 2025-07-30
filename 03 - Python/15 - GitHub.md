##### Étapes pour un premier `git push` vers GitHub :

1. 📦 Initialise ton dépôt local (si ce n’est pas déjà fait)
``` bash
cd mon-repertoire
git init
```

2. 📝 Ajoute tes fichiers et fais un commit
``` bash
git add .
git commit -m "Initial commit"
```

3. 🌐 Crée un repo vide sur GitHub
Va sur [https://github.com](https://github.com)  
– Donne-lui un nom (ex: `export-engine-v2`)  
– Laisse vide (ne coche pas README, .gitignore, etc.)

4. 🔗 Connecte ton dépôt local au dépôt GitHub distant
``` bash
git remote add origin https://github.com/USERNAME/REPO_NAME.git
```
5. 🚀 Envoie ton code sur la branche `main`
``` bash
git branch -M main
git push -u origin main
```
##### Pour bien faire un `push` quotidien :

1. Tu ajoutes les nouveaux fichiers/modifs :

    `git add .`

2. Tu commits avec un message clair :

    `git commit -m "Ajout de l'interface ILogProvider et début du module export"`

3. Tu pushes sur GitHub :

    `git push origin main`


> 💡 Si tu fais ça chaque jour, **aucun code n’est écrasé**. Git garde **tous les états précédents** dans les commits.