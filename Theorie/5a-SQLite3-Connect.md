Dans le cadre de ce cours, nous utiliserons une base de données SQLite à laquelle nous accéderons depuis le module Python 'sqlite3'.
# Prérequis
- Savoir modéliser une petite base de données
- Connaissances de base en SQL : 
	- SQL DDL : création du schéma d'une base de données (création de tables).
	- SQL DML : ajout / modification / suppression d'enregistrements.
	- SQL DQL : rechercher des informations dans la base de données.
- Utilisation de SQLite
**Rem** : ces prérequis sont détaillés dans le cours de 4ème année : 'Base de données relationnelle'.
# References
- Doc Python : [sqlite3 - Interface DB-API pour bases de données SQLite](https://docs.python.org/fr/3.14/library/sqlite3.html)
- Geeksforgeeks : [Python SQLite](https://www.geeksforgeeks.org/python/python-sqlite/)

# Le module SQLite3

## 1. Importer SQLite3
Le module `sqlite3` fait partie de la libraire standard de Python. 
Il ne faut donc rien télécharger / rien installer.
Il suffit juste de l'importer dans notre projet.
```python
import sqlite3
```
## 2. Etablir une connection à la DB
Pour interagir avec la base de données; il nous faut créer un *objet* 'connexion'.
Celui-ci s'obtient en appelant la *méthode* `connect()` sur l'objet `sqlite3`, et en lui passant comme argument le chemin vers la base de données SQLite.

*Création d'une connexion à une DB nommée 'taches.db'*
```python
conn = sqlite3.connect('taches.db')
```
**Rem** : n'utilisez pas ce code tout de suite... mais lisez (et utilisez plutôt) ce qui suit.
### Problème : gestion de la concurrence
Comme évoqué dans le cours DB sur SQLite :
- une DB SQLite n'accepte qu'une seule requête en écriture à la fois.  Il est donc important, au terme de chaque requête, de s'assurer que la connexion à la base de données soit bien fermée au terme de la requête pour permettre à d'autres requête d'écrire à leur tour, sans qu'un LOCK ne vienne verrouiller la DB.
- Flask autorise plusieurs requêtes HTTP simultanées : on doit s'assurer que les variables utilisées par ces requêtes soient isolées les unes des autres.
### Solution : l'objet `g` dans Flask
L'objet `g`, dans le framework Flask :  est un objet propre à chaque requête HTTP en cours.
Il sert de stockage *global* pour les données utilisées par la requête le temps que dure celle-ci.
- Il est propre à chaque requête => 2 requêtes différentes ne partageront pas les mêmes valeurs.
- Il est *global* au sein de la requête => les différentes fonctions (routes) pourront l'utiliser.
- Il est détruit quand la requête se termine => il libère les ressource qu'il utilise.

Ceci est particulièrement utile pour s'assurer que la connexion à la base de donnée sera libérée lorsque la requête se termine.

*Importer l'objet 'g', depuis le module Flask*
```python
from flask import g   # Pour pouvoir utiliser l'objet g, il faut l'importer.
```

*Stocker la connexion vers la DB dans l'objet 'g'*
```python
DATABASE = 'instance/<nom_de_la_db>'  # Chemin vers le fichier de la DB
def get_db():
    if 'db' not in g:
	    # Crée une connexion à la DB et stocke celle-ci dans l'objet 'g'
        g.db = sqlite3.connect(DATABASE)
        # Permet d'accéder aux résultats de la requête via le nom des attributs
        g.db.row_factory = sqlite3.Row 
    return g.db
```
**Rem** : on stockera les fichiers 'sensibles' dans un répertoire **instance/** de notre projet.
**Attn** : on veillera à NE JAMAIS transmettre ces informations sensibles sur Github.  On les sauvegardera indépendamment dans un espace sécurisé.

Désormais, quand on aura besoin d'accéder à la base de données; il nous suffira d'appeler la fonction `get_db()`; laquelle nous renverra un objet *connexion* vers celle-ci.

## Quelques remarques additionnelles 
### Champs de formulaire vides et valeur `NULL`
Lors de la transmission des données d'un formulaire; il peut arriver que certains champs ne soient pas remplis par l'utilisateur.  La valeur extraite de ceux-ci est évaluée à la chaîne vide : `''`.
Or, SQLite ne convertit par la chaîne vide en valeur `NULL` (qui représente, l'absence de valeur pour un attribut, dans une base de données).
En revanche, SQLite convertit la valeur `None` en valeur `NULL`.

Pour remédier à ce problème; l'on utilisera la syntaxe suivante (qui n'est autre qu'un `if... else` condensé sur une seule ligne).

```python
if request.method == 'POST':
    nom = request.form.get('nom') if request.form.get('nom') != '' else None
```
**Rem** : dans notre exemple; on utilise un requête `POST`, transmise depuis un formulaire qui contient un champ 'nom', dont on veut récupérer la valeur.


