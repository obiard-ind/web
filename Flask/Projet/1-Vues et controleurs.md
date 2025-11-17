Dans un premier temps, nous avons vu comment Flask nous permet de répondre à une URL (qu'elle soit transmise via la barre d'adresse du navigateur, ou via un formulaire).

Le but de cet exercice (et des suivants); est de vous guider dans la réalisation du projet pour ce cours de **Web**.  Pour ce faire, il mettra en oeuvre les connaissances théoriques que vous aurez acquises en lisant les fiches de théorie afin de les transformer en nouvelles compétences utiles pour la suite du projet.

Un des objectifs de ces exercices (et, plus globalement, du projet) est également l'**autonomie**.
N'hésitez donc pas à faire vos propres recherches (Exemple : "comment capture une requête GET en Flask", "dans quel cas utiliser une requête GET et une requête POST", "comment vérifier si une donnée est présente",...).

# Exercice 1 : création d'une liste de tâches.
1. Créez un page html (à l'url /tache/ajouter) comprenant un petit formulaire.  Celui-ci demandera à l'utilisateur d'introduire un bref descriptif de la tâche (max. 50 caractères), d'un descriptif plus complet, d'une date de rappel et d'une échéance finale.
2. Dans le contrôleur, crééz une route et sa vue (la fonction associée qui va traiter la requête) qui vont enregistrer les tâches transmises par le formulaire dans une liste de tâches.
   Rem : utilisez les structures de données appropriées... 

# Exercice 2 : affichez votre liste de tâches
1. Créez une nouvelle route qui renverra au navigateur la liste des 'bref descriptifs' des tâches enregistrées.
2. Utilisez à présent un *template* pour renvoyer sous une forme agréable à lire, les informations relatives aux tâches enregistrées.




