#DML Par DML (Data Manipulation Language), on entend les requêtes SQL qui vont manipuler les données; à savoir typiquement :
- `INSERT` : pour l'ajout d'enregistrements
- `UPDATE` : pour la mise à jour des valeurs d'enregistrements
- `DELETE` : pour la suppression d'enregistrements

Ces requêtes suivent toujours le même schéma :
1. Obtenir une connexion à la base de données
2. Créer un objet 'curseur' au départ de cette connexion
3. Créer une requête paramétrée
4. Exécuter la requête.
5. Sauvegarder les changements dans la base de données.

# 1. Obtenir une connexion à la base de donnée
Voir : [5-SQLite3-Connect](5-SQLite3-Connect.md)
# 2. Créer un objet curseur sur la connexion
Voir : [5a-SQlite3-Cursor](5a-SQlite3-Cursor.md)
# 3. Les cas des requêtes DML
Contrairement aux requêtes de type `SELECT`; lesquelles ne font que récupérer les données; les requêtes `INSERT`, `UPDATE` ou `DELETE` modifient quant à elles le contenu de la base de données.

Deux instructions seront donc à considérer dans le cadre de leur usage.  
Toutes deux sont appelées sur la connexion (et non sur le curseur)
- La méthode `.commit()` : celle-ci donne instruction à la DB de sauvegarder les changements.
- La méthode `.rollback()` : celle-ci donne instruction à la DB d'annuler les changements.

## La notion de *transaction*
#def une **transaction** est un ensemble d'opérations SQL qui doivent être considérées comme un tout.

Une transaction peut être ainsi composées de plusieurs requêtes SQL (`INSERT`, `UPDATE`, `DELETE`) considérées comme **une seule unité** de traitement.

Elles permettent de **garantir l'intégrité des données** en respectant un principe nommé <u>ACID</u>
(Atomicité, Consistence, Isolation, Durabilité); que nous ne détaillerons pas ici.

Dans ce cadre :
- Soit le `commit` applique tous les changements demandés par les requêtes qui constituent la transaction.
- Soit le `commit` échoue; et tous les changements demandés par les requêtes qui constituent la transaction son annulés; y compris ceux qui avaient réussi.  Cette annulation d'une transaction s'appelle un `rollback` (retour en arrière)

## Commit (sauvegarder les modifications)
L'objet *curseur* transporte nos requêtes du programme Python vers la DB; et donne instruction à celle-ci de les exécuter.

Dans le cas des requêtes DML; une étape supplémentaire est requise : donner instruction à la DB d'enregistrer les demandes modifications véhiculées par le curseur.  Ceci s'effectue à l'aide de la méthode `commit()`; laquelle s'exécute <u>sur l'objet <i>connexion</i></u>.
## Rollback (annuler les modifications)
Comme nous l'avons évoqué au chapitre précédent ([5a-SQLite3-DML](5a-SQLite3-DML.md)); il peut arriver que des erreurs se produisent lors de l'exécution d'une requête !
Ceci est particulièrement dommageable; s'il s'agit d'une requête qui modifie les données.
Pour éviter que cela n'arrive; la méthode `rollback()` sera appelée <u><i>sur l'objet connexion</i></u> si exception se produit.
La méthode `rollback()` annule la transaction et restaure la DB dans l'état qui précédait la transaction.

## Exemple de transaction avec commit et rollback

```python
try:
    conn = get_db()      # Connexion à notre DB
    cur = conn.cursor()  # Création de l'objet 'curseur'
    # Exécution d'une première requête : ajout d'un enregistrement
    query = "INSERT INTO utilisateurs (username, password) VALUES (?,?)"
	values = (username, hashed_password)
	cur.execute(query,values) # Exécution de la requête paramétrée
	# Exécution d'une seconde requête
	query = "UPDATE utilisateurs SET email = new_email WHERE username = ?"
	values = (username,)
	cur.execute(query, values)
	# Appliquer les modifications demandées dans la DB 
	cur.commit()         # Sauvegarde des modifications dans la DB
	conn.close()         # Fermer la connexion à la DB pour libérer le verrou.
except sqlite3.Error as e: 
    print(f"Une erreur s'est produite : {e}")
    # Annuler les changements
    conn.rollback()    
```

On notera plusieurs choses dans cet exemple :
- On peut passer plusieurs requêtes à l'objet *curseur* avant de faire un `commit` !
- Le `commit` réussi si toutes les requêtes qui sont passées à la DB réussissent.
- En cas d'échec d'une seule de ces requêtes; le `commit` échoue.  Une *exception* est levée; et tous les changements qui constituent la transaction sont annulés via le `rollback`.







