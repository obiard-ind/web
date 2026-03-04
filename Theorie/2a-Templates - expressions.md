Nous avons vu juqu'à présent, comment renvoyer le contenu d'une page HTML à l'aide de la fonction [[2-Templates - Intro#La fonction *render_template()*|render_template()]].

Le contenu de cette page étant constitué de code HTML et CSS destinés à présenter une information **statique**.

Or, les templates peuvent également servir à afficher du contenu **dynamique**.

En Flask, le moteur de templates s'appelle **"JinJa"**.
Il permet d'insérer des **variables** et des **structures de contrôles** (boucles, structure conditionnelles) dans fichier texte, tel que des pages HTML, pour créer des contenus qui dépendront de valeurs qui seront transmises au template lors de l'appel de celui-ci. 

# Les expressions : {{ ... }}

Dans un template Flask, les expressions sont délimitées par des doubles accolades : **{{ ... }}**

Celles-ci peuvent être de n'importe quel type Python : simple (string, int, bool,...) mais aussi plus complexe (list, dict, tuple,... ) et même des *objets*.

Pour calculer la valeur d'une expression; des variables sont transmises au *template* comme argument de la fonction [[2-Templates - Intro#La fonction *render_template()*|render_template()]] lors de la génération du document à renvoyer depuis une [[1-Application basique#Les routes url <-> vue|route]].

```python
@app.route('/utilisateur/<nom>')
def utilisateur(nom):
    render_template("utilisateur.html",nom = nom )
```

*utilisateur.html*
```
Bienvenue sur le site : {{ nom }} 
```
**Rem** : pour plus de lisibilité dans cet exemple, nous avons omis ici le code HTML.

## *Route* avec une partie variable

Vous aurez peut-être remarqué que cette *route*, et la définition de fonction qui l'accompagne sont un peu différentes de celles que nous avons utilisées jusqu'à présent...  A savoir que :
- la route contient une partie entre `< >` 
- la fonction contient désormais un paramètre.

### > la route
Les crochets :  `< >` , insérés au niveau de la route, permettent de considérer que la partie concernée dans le *chemin* peut varier au niveau de l'URL.

Ainsi, dans notre exemple `@app.route('/utilisateur/<nom>')`,
cette route 'capturera' toutes les requêtes passées sur les chemins suivants :
```
/utilisateur/jonathan
/utilisateur/nicole
/utilisateur/antoine
etc...
```
... et elle affectera à la variable *nom*, la partie variable de notre URL.

### > la fonction associée à la route
Rem : on utilisateur également le terme générique **endpoint** pour qualifier cette fonction.
#def Un **endpoint** est l'identifiant (le nom) qui détermine l'unité logique (la fonction) qui va prendre en charge la requête associée à la route.  

Comme nous venons de le voir, celle-ci pourra disposer de paramètres dont la valeur dépendra de l'URL transmise par le navigateur à la route.

Dans notre exemple; la *route* assigne la partie variable de l'URL à la variable `nom`
Et cette variable est définie comme paramètre au niveau de notre fonction.
**Attn** : le nom du paramètre et le nom de la variable définie au niveau de la route doivent correspondre; afin de permettre de récupérer la valeur tirée de l'URL dans la fonction.

Les paramètres de la fonction pourront ainsi être utilisés dans le corps de la fonction comme pour n'importe quelle fonction Python.

```python
@app.route('/utilisateur/<nom>')    # nom désigne la partie variable de l'URL
def utilisateur(nom):               # nom désigne le paramètre de la fonction.
    # code de la fonction
```
### > la fonction render_template()
Comme nous l'avons déjà évoqué; la fonction [[2-Templates - Intro#La fonction *render_template()*|render_template()]] permet de générer un document comportant des parties statiques et des parties dynamiques.

La <u>premier paramètre</u> de notre fonction *render_template* est le chemin qui conduit vers ce template ! 
**Rem** : par défaut, il s'agit des documents situés à l'intérieur du sous-répertoire **templates/** 

Les <u>paramètres suivants</u> sont les variables passées à ce template.
- Le nom de la variable : est réutilisé au sein du template entre les `{{ }}` 
- Il est courant d'assigner au paramètre sa valeur; sous la forme d'une expression du type : 
  `<nom_du_paramètre> = <valeur_du_paramètre>`

```python 
render_template("utilisateur.html",nom = nom )
```

### > le fichier template
Dans le fichier template correspondant; on pourra utiliser le nom des paramètres de la fonction render_template() pour en récupérer les valeurs et utiliser ces dernières dans des expressions.

**Rem** : on notera dans l'exemple suivant que l'on utilisera également des variables définies au sein de la fonction.  Ici, la variable `points`, un *dictionnaire* des points des élèves.

```python
@app.route('/notes/<eleve>)
def note_eleve(eleve):
	points = {'Sylvie':18,'Alain':14,'Auguste':9,'Bernard':11,'Gilbert':13}
	return render_template('resultats.html',eleve = eleve, points = points)
```

Exemple de fichier template : *resultats.html*
```Jinja
L'élève {{ eleve }} a obtenu {{ points.get(eleve) }} points. 
```

Toujours sur notre serveur local, à l'adresse `http://localhost:5000/notes/Bernard`
... le serveur nous renverra : `L'élève Bernard a obtenu 11 points`


