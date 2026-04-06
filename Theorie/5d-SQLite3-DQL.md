#DQL Par DQL (Data Query Language), on entend les requêtes SQL qui vont aller interroger la base de données et renvoyer un résultat.  Typiquement, il s'agit des requêtes `SELECT`

Ces requêtes suivent le schéma suivant :
1. Obtenir une connexion à la base de données
2. Créer un objet 'curseur' au départ de cette connexion
3. Créer une requête paramétrée
4. Exécuter la requête.
5. Récupérer le résultat de la requête.

# Exécution d'une requête `SELECT`

Le principe est globalement le même que celui des requêtes DML; à deux différence près :
- il n'y a pas besoin de `commit` pour valider les changements, ni de `rollback` pour annuler ceux-ci en cas d'échec de la transaction.  Et c'est normal, vu que les requêtes `SELECT` ne modifient pas les données.
- la requête est exécutée directement lors de sa réception par la base de donnée; laquelle renvoie immédiatement un résultat au travers de l'objet *curseur*.  

## Exemple 1 

*Récupération des nom, prenom et adresse email de tous les utilisateurs*
```python
try:
    conn = get_db()      # Connexion à notre DB
    cur = conn.cursor()  # Création de l'objet 'curseur'
    query = "SELECT nom,prenom,email FROM utilisateurs"
	values = (,)
	cur.execute(query,values) # Exécution de la requête paramétrée
	conn.close()         # Fermer la connexion à la DB pour libérer le verrou.
except sqlite3.Error as e: 
    print(f"Une erreur s'est produite : {e}")  
```

**Rem** : vous noterez qu'en l'absence de paramètre `?` dans cette requête; le tuple de valeurs pour s'y substituer est vide.  Un tuple vite s'écrit `(,)`.  La virgule est là pour indiquer qu'il s'agit d'un *tuple*; et non du parenthésage d'une expression vide.
L'on aurait également pu écrire l'exécution de la requêtes sous la forme suivante :

```python
# ne ps oublier la virgule pour indiquer l'absence de valeurs en second argument.
resultat = cur.execute(query,) 
```

## Exemple 2

*Récupération des informations de l'utilisateur connecté 
```python
# Récupération du 'username' de l'utilisateur via l'objet 'session'
username = session.get('username')
try:
    conn = get_db()      # Connexion à notre DB
    cur = conn.cursor()  # Création de l'objet 'curseur'
    query = "SELECT nom,prenom,email FROM utilisateurs WHERE username = ?"
	values = (username,) 
	cur.execute(query,values) # Exécution de la requête paramétrée
	conn.close()         # Fermer la connexion à la DB pour libérer le verrou.
except sqlite3.Error as e: 
    print(f"Une erreur s'est produite : {e}") 
```

**Rappel 1**  : le 'username' dans la requête et le nom de l'attribut figurant dans la table utilisateurs; tandis que le 'username' dans le tuple des valeurs; est le nom de la variable qui contient le nom d'utilisateur pour lequel nous voulons récupérer les valeurs des attributs 'nom','prenom' et 'email'.

**Rappel 2** : notez à nouveau la syntaxe d'un tuple contenant une seule valeur ! Un tuple, contient toujours au moins un virgule.
# Récupérer les lignes du résultat de la requête
Le résutat retourné par l'exécution de la méthode `cur.execute()` est un objet `curseur` !
Il ne s'agit pas d'un tuple, ou d'une liste de tuples correspondant aux données souhaitées.

Pour pouvoir obtenir ces données sans la forme souhaitée; on utilisera, sur cet objet; l'une des méthodes suivantes.
## `fetchone()`
```python
cur.execute()
une_ligne = cur.fetchone()
```
Retourne un *tuple* contenant les valeurs du prochain enregistrement dans le résultat obtenu.
## `fetchall()`
```python
cur.execute("SELECT * FROM taches")
toutes_les_lignes = cur.fetchall()
```
Retourne une *liste de tuples*; ou chaque *tuple* contient les valeurs d'un enregistrement.

## `fetchmany(n)`
```python
cur.execute("SELECT * FROM taches")
prochaines_lignes = cur.fetchmany(5) 
```
Retourne une *liste de tuples*. La liste est constituée des valeurs des `n` prochains résultats.  

#### Comparaison des différentes méthodes

| Méthode        | Type de résultat                       | Valeur si aucun résultat |
| -------------- | -------------------------------------- | ------------------------ |
| `fetchone()`   | Un tuple (une ligne)                   | `None`                   |
| `fetchmany(n)` | Une liste de `n` tuples (n lignes)     | `[]`                     |
| `fetchall()`   | Un liste de tuples (toutes les lignes) | `[]`                     |

# Accéder aux attributs de chaque ligne
Une fois le résultat obtenu dans une structure de données bien connue (liste ou tuple); on pourra lui appliquer le traitement que l'on souhaite.

Typiquement, on parcourera les lignes tour à tour.  Et pour chacune, on accèdera aux valeurs qu'elles contiennent pour les traiter (En général, pour les afficher).

On pourra accéder à ces valeurs de deux façons possibles :
- soit par leur rang (leur index dans le tuple)
- soit par leur nom
## Accéder aux valeurs par leur rang
#todo

## Accéder aux valeur par leur nom
#todo
