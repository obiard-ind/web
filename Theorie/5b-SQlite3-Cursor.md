# Créer l'objet 'curseur'
Après avoir établit une connexion à notre base de données; nous aurons besoin d'un *objet* nous permettant :
- d'**exécuter des requêtes** (création, modification, sélection)
- d'en récupérer et d'en **parcourir les resultats**.
Cet objet est appelé un **curseur**, et nous le créerons en appelant la méthode `cursor()` sur l'objet 'connexion' que nous venons de créer.

*Création du 'curseur' au départ de la connexion vers notre DB*
```python
conn = get_db()   # On fait appel à notre fonction de connexion définie plus haut.
cur = conn.cursor()
```

Vous pouvez vous représenter :
- la connexion `conn`, comme un pont entre Python et votre DB SQLite
- le curseur `cur`, comme une navette entre les deux rives; qui transporte les instructions (requêtes) du programme Python vers la DB; et en récupère les résultats qu'il transporte en sens inverse; de la DB vers le code Python.

<figure>
   <img src="https://content.codecademy.com/programs/data-science-path/relational-databases/DataScienceArticle_Animation_2.gif">
   <figcaption>Source : https://content.codecademy.com/programs/data-science-path/relational-databases/DataScienceArticle_Animation_2.gif</figcaption> 
</figure>
# Exécuter notre requête
Après avoir défini notre objet *curseur*, on peut à présent lui passer des requêtes à exécuter.
Il peut s'agir de n'importe quelle requête SQL 
- DDL (`CREATE`, `DROP`,...)
- DML (`INSERT`, `UPDATE`, `DELETE`) 
- DQL (`SELECT`)

## Requête paramétrée
Une requête paramétrée est simplement une requête dans laquelle on aura remplacé les valeurs par des `?` dans la chaîne qui constitue la requête. 

Les valeurs seront transmises sous forme de *tuple*; lors de l'exécution de la requête; et se substitueront aux `?`; dans leur ordre d'apparition.  Pour faire le parallèle avec les fonctions : le *tuple* de valeurs correspond aux arguments; et les `?` font office de paramètres.

Cette technique permet de prévenir une technique de hacking, appelée [SQL injection](https://www.w3schools.com/sql/sql_injection.asp)

Pour bien distinguer la requête paramétrée des valeurs qui vont se subsituer aux paramètres `?`; je vous suggère de stocker l'une et l'autre dans des variables différentes :
```python
query = "INSERT INTO utilisateurs (username, password) VALUES (?,?)"
values = (username, hashed_password)
```
- Dans la variable `query`; `username` et `password` font référence aux nom des attributs de la table `utilisateurs`
- Dans la variable `values` : `username` et `hashed_password` font référence à deux variables; qui contiennent respectivement un nom d'utilisateur et un mot de passe *hashé* (crypté) préalablement définis.

## Exécution de la requête paramétrée
Exécuter la requête se fera simplement en appellant la méthode `.execute()` sur l'objet curseur que nous avons créé précédemment.  Celle-ci prendra deux arguments : 
- La requête paramétrée
- Le *tuple* de valeurs

```python
cursor.execute(query,values)
```
# Gérer les erreurs (Exceptions)
#def Une **exception** est une erreur qui survient <u>durant l'exécution du programme</u> et qui; si elle n'est pas gérée, mène à l'arrêt brutal de celui-ci.

Dans le cadre de l'utilisation d'une base de donnée; il n'est pas rare de rencontrer ce type d'erreur.
Elle peuvent survenir pour diverses raisons :
- parce que nous n'avons pas pris assez de précautions pour nous assurer que les valeurs que nous souhaitons insérer correspondent bien au type de données de l'attribut; ou ne respectent pas les contraintes imposées sur celui-ci (`NOT NULL`, `UNIQUE`,...)
- parce que la base de données est inaccessible (service indisponible, problème de réseau,...), ou qu'elle est verrouillée par une autre opération (LOCKed)
- etc...

Or, il est **éminemment important de gérer ce type d'erreur** :
- pour <u>éviter de *planter* notre programme</u>; et que celui-ci ne devienne inaccessible et doive être redémarré
- pour <u>informer l'utilisateur</u> que quelque chose s'est mal passé et que le résultat n'est pas celui attendu ! Imaginez que l'ajout d'une commande échoue et que l'utilisateur n'en soit pas informé et croie que celle-ci a bien été encodée...
- mais surtout pour <u>s'assurer que la base de données reste dans un état cohérent</u>; notamment en évitant des ajouts partiels (une partie de la requête ayant réussi; et l'autre pas...).  Comment faire confiance aux données enregistrées dans ce cas ?

La bonne nouvelle; c'est que nous pouvons **gérer ce type d'erreurs** quand elles surviennent; et même, de **définir une réponse adpatée** selon les cas.

Dans le cadre de ce cours, nous nous limiterons juste à **capturer l'exception** ou l'erreur :
- pour en informer l'utilisateur
- pour nous assurer que notre DB reste intègre.
## L'instruction `try... except`
La stratégie pour gérer les exception est simple :
- exécuter nos instructions à l'intérieur d'un bloc `try`
- capturer les erreurs éventuelles (on parle d'exception) pour pouvoir les gérer, dans le bloc `except`

*séquence d'instructions complète pour un exemple de requête INSERT*
```python
try:
    conn = get_db()      # Connexion à notre DB
    cur = conn.cursor()  # Création de l'objet 'curseur'
    query = "INSERT INTO utilisateurs (username, password) VALUES (?,?)"
	values = (username, hashed_password)
	cur.execute(query,values) # Exécution de la requête paramétrée
	cur.commit()         # Sauvegarde des modifications dans la DB
	conn.close()         # Fermer la connexion à la DB pour libérer le verrou.
except sqlite3.Error as e: 
    print(f"Une erreur s'est produite : {e}")  
    conn.rollback()
```

### Explications :
Si une erreur survient lors de l'exécution de la requête (séquence d'instructions du bloc `try:`) **et** que cette exception est en rapport avec la gestion de notre DB (via le module `sqlite3`),
Alors :
- On affichera un message d'erreur sur le terminal 
  *Note* : ce message contiendra le détail sur l'exception; à travers le nom de la variable qui l'enregistre (par convention, on choisit souvent le nom `e` pour désigner une exception)
- On annulera toutes les modifications survenues depuis le dernier `commit` valide; pour restaurer la DB dans son dernier état cohérent connu.  Ceci s'effectue en exécutant la méthode `.rollback()` sur l'objet qui représente la connexion; ici : `conn`.

**Rem** : nous verrons plus en détail ce que signifient `commit` et `rollback` dans la suite consacrée aux instructions DML (`INSERT`, `UPDATE`, `DELETE`)


L'exécution de notre requête se fait toujours en deux temps :
1. On passe des requêtes à notre objet **curseur** => méthode : `.execute()`
2. On valide ces requêtes via l'objet **connexion** => méthode : `.commit()`
**Rem** : on peut passer plusieurs requêtes à l'objet 'curseur'; avant de ne valider qu'une seule fois l'ensemble des requêtes passées.

```python
try:
    cur.execute("INSERT INTO utilisateurs (username,passowrd) VALUES (?,?)",(username,hashed_password))
    conn.commit()
except sqlite3.Error as e:
    conn.rollback()
```

**Note 1** : on notera la syntaxe un peu particulière de la requête `INSERT INTO`; laquelle remplace les valeurs par des `?` dans la chaîne qui correspond à la requête; suvie ensuite les valeurs qui y seront substituées.  
**Note 2** : on placera l'exécution de la requête dans un block `try... except` qui permettra, en cas d'erreur lors de l'exécution, d'annuler tous les changements liés à ce `commit`.



