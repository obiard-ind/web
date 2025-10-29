# Chapitres concernés
[[2-Templates - Intro]]
[[2a-Templates - expressions]]
[[2b-Templates - structures de contrôle]]

# Exercices

## Ex1 : capitales - ajouter et supprimer des entrées

#### Durée proposée : 3h
#### Objectif poursuivi :
- Utiliser les expressions et les structures de contrôle dans les templates pour renvoyer un message pertinent à l'utilisateur.
- Faire usage de structures de données (liste, dictionnaire,...) au niveau **backend** (côté *serveur*) afin d'enregistrer et supprimer des instructions passées au serveur via l'URL.
#### Exercice :
Dans la fiche dédiée aux [[2b-Templates - structures de contrôle | structures de contrôle]], vous avez vu comment afficher la liste des pays et de leur capitales depuis un dictionnaire Python défini comme variable globale.

Dans cet exercice; il vous sera demandé de créer de nouvelles routes permettant d'ajouter de nouvelles entrées dans ce dictionnaire; et d'en supprimer d'autres.

Les routes est les templates associés que vous utiliserez seront nommés comme suit :

| Route                                 | Template              |
| ------------------------------------- | --------------------- |
| `/capitales/add/<pays/<capitale>`     | capitales_add.html    |
| `/capitales/delete/<pays>/<capitale>` | capitales_delete.html |

Tant lors de l'ajout que de la suppression; vous devrez renvoyer un message à l'utilisateur pour confirmer que la commande a bien été exécutée.  Dans le cas contraire, l'utilisateur devra aussi en être notifié ! Les cas suivants devront notamment être couverts :
- Erreur d'insertion lorsque le pays **ou** la capitale existent déjà dans le dictionnaire.
- Erreur lors de la suppression lorsque l'association du pays et de la capitale n'existe pas dans le dictionnaire !  Par exemple, avec les chemins suivants dans l'URL :
	- `/capitales/delete/Russie/Moscou`
	- `/capitales/delete/Belgique/Madrid`
Pour le dictionnaire des pays et capitales suivants :
```
  capitales = {
      "Belgique":"Bruxelles",
      "Espagne":"Madrid",
      "Tunisie":"Tunis"
  }
  ```

A la suite du message de confirmation ou d'échec que vous viendrez d'afficher; vous afficherez également la liste actualisée des associations pays/capitales.



