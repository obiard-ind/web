# Prérequis
Avant de commencer le projet; assurez-vous :
- d'avoir créé le répertoire à l'endroit exact suivant sur votre PC : `Documents\5tt\web\projet`
- d'avoir accepté l'invitation qui vous a été envoyée par mail à rejoindre dépôt Github au travers duquel vous collaborerez
# Configuration du dépôt Git
## Initialisation du dépôt et association avec Github
Les commandes suivantes à exécuter en *ligne de commande*.
Vous trouverez les explications relatives à celles-ci dans la [[0-Presentation du contenu|documentation relative à git]].
**Rem** : Vous pouvez également utiliser l'outil graphique que vous voulez pour gérer vos dépôts.  Cette documentation est juste une aide pour vous aider à démarrer dans le projet; et vous introduire aux commandes de base de Git via la ligne de commande.

Les commandes suivantes sont à exécuter au départ de répertoire `Documents\5tt\web\projet` 
1. Initialiser votre dépôt : `git init`
2. Associer votre dépôt au dépôt distant de votre groupe sur Github :
   `git remote add orign <url du dépôt>`
   **Rem** : vous trouverez l'URL du dépôt en :
	1. vous connectant à Github
	2. Aller sur Code : ![[Pasted image 20251117123920.png]]
	3. Copiant l'URL correspondant au dépôt du groupe
	   ![[Pasted image 20251117124012.png]]


## Ajout d'un fichier .gitignore
La présence d'un fichier `.gitignore` à la racine du dépôt permet d'indiquer quels fichiers / répertoires ne pas enregistrer dans les commits.

Typiquement, on n'enregistrera dans les commits que les fichiers qui contiennent le code source.
On excluera donc les fichier suivants :
- Les fichiers temporaires
- Les fichiers compilés
- Les gros fichiers / répertoires qui contiennent des programmes / librairies externes 
**Très important** : n'envoyez jamais sur Github des fichiers qui contiennent des données sensibles (adresses email, mots de passe, données personnelles ou données utilisateur,...). 

Créez donc, à la racine de votre projet, un fichier `.gitignore` qui contiendra les fichiers / dossiers à exclure des commits.
1. Ouvrez un terminal à la racine de votre projet `Documents\5tt\web\projet`
2. Créez un fichier nommé `.gitignore`
3. Placez dans celui-ci les lignes suivantes (vous pourrez en ajouter d'autres par la suite, selon les besoins)
*fichier .gitignore*
```
# Ignorer le répertoire venv (environnement virtuel)
venv/
# Ignorer les fichiers Python compilés
__pycache__/
*.py[cod]
*.pyo
```
4. Ajoutez celui-ci à vos commits : `git add .gitignore`
5. Créez un commit avec cette modification : `git commit -a -m "Ajout d'un .gitignore"`
6. Poussez votre commit sur Github : `git push origin <nom_branche>`
   Rem : `<nom_branche>` est le nom de la branche vers laquelle vous poussez; et qui correspond au nom de la branche locale dans laquelle vous vous trouvez.  Pour connaître cette branche : `git status`.  

**Bravo !** Vous avez réalisé votre premier commit et l'avez *poussé* sur votre dépôt Github !

## Création de branches
A l'initialisation; votre dépôt aura une *branche* principale : `main`.

Afin d'éviter les *conflits* (ce qui arrive quand un même fichier est modifié par plusieurs utilisateurs en même temps); je vous recommande de créer une branche pour chaque participant.

Ceci permettra d'éviter les conflits; tout en ayant accès au code des autres membres du groupe.

1. Ouvrez un 'Terminal' au départ de `Documents\5tt\web\projet`
2. Vérifiez que votre dépôt est bien configuré : `git status`
3. Créer une branche à votre nom (ou prénom) : `git branch <prenom>`
   Rem : mettez juste le nom ou le prénom, sans les `<>` autour !
4. Basculez sur cette branche afin de la rendre active : `git checkout <prenom>`
5. Vérifiez que vous êtes sur la branche souhaitée : `git status`



