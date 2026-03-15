# Prérequis : partir d'un code commun avec Git
Pour cette seconde remise; vous allez partir du code que vous avez déjà réalisé pour première remise; et faire évoluer celui-ci pour le rendre conforme aux consignes du cahier des charges pour la seconde remise.

Pour cela, il faudra que chaque membre du groupe parte de la même base de code.
Git est là pour vous aider.
### Etape 1 : fusionner le dernier code fonctionnel avec `main`
Identifiez la branche qui contient le dernier code fonctionnel de votre projet.  Normalement, il devrait s'agir de la branche qui a servi à l'extraction du fichier .zip pour la première remise.
**Tip** : si vous avez suivi les consignes; elle devrait s'appeller `remise1`; mais il se peut que vous lui ayez donné un autre nom (par exemple : le prénom d'un des particpants du groupe)

Assurez-vous que c'est bien à partir de ce code que vous voulez continuer de travailler : 
- `git checkout <nom_de_la_branche>` : basculer vers la branche souhaitée
- `git pull origin <nom_de_la_branche>` : tirer les dernières modifications depuis Github.
- Exécutez-votre code; testez-le... pour vous assurer qu'il s'agit de la version qui va servir de base pour la suite.
- `git checkout main` : basuclez vers la branche 'main' (la branche principale).
- `git merge <nom_de_la_branche>` : fusionne le code de la branche avec la branche principale.
- `git branch remise2` : on crée une nouvelle branche nommée `remise2`.  Celle-ci nous servir de point de départ pour l'enregistrement du code à partir de ce point.
- `git checkout remise2` : on bascule vers cette branche.

## Etape 2 : créer une nouvelle branche par utilisateur
Git est un outil puissant; mais qui nécessite du temps pour être bien pris en main.
En tant qu'apprenants; vous rencontrerez quelques frustrations avec celui-ci.... et c'est normal !

Pour que chacun puisse participer au code; et faire ses 'erreurs' sans impacter les autres; je vous propose de créer, comme pour cette première remise, une nouvelle branche pour chacun des participants.

- `git checkout remise2` : assurez-vous de bien démarrer depuis la branche `remise2` que vous venez de créer (cf. supra)
- `git branch <nom_utilisateur>` : je vous propose d'utiliser votre prénom pour nommer la branche.  Si celui-ci existe déjà (par exemple, parce que vous avez déjà une branche nommée ainsi); ajoutez-lui le suffixe '2', par exemple pour le distinguer premier (eg. `git branch bruno2`)
- `git checkout <nom_utilisateur>` : pour rendre cette branche active sur votre PC.
- `git pull origin <nom_utilisateur>` : pour rendre cette branche disponible sur votre dépôt Github.

## Etape 3 : sauvegarder vos modification sur Github

A partir de là, vous pouvez coder, enregistrer votre code dans des 'commits' locaux, et sauvegarder ceux-ci sur Github.  Petit rappel des commandes :
- `git status` : pour voir les fichiers modifiés / ajoutés / supprimés.
- `git add <nom_du_fichier>` : pour ajouter un fichier au futur 'commit' (sauvegarde)
  ou `git add .` : pour ajouter toutes les modifications au futur 'commit'.
- `git commit -a -m <court message descriptif des modifications>`
- `git push origin <nom_branche>` : pour pousser vos modifications sur Github afin des les protéger contre l'effacement; et les rendre accessibles aux autres membres du groupe.
  **Rem** : `<nom_branche>` doit correspondre au nom de la branche locale sur laquelle vous travaillez.
