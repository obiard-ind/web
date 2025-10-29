# Création de l'application

```python
from flask import Flask
app = Flask(__name__)
```
Ces deux lignes créent une application web que nous avons décidé d'appeler *app* !
# Routes et Fonctions de vues

## Principe d'une requête web
1. Un client web (navigateur), envoie une requête à un serveur.
2. Le serveur traite la requête.
3. Le serveur renvoie la réponse au client.

Nous nous intéresserons dans la suite de ce cours à la façon dont le serveur web va traiter les demandes du client, pour afficher le contenu demandé.
## Les routes
Quand un utilisateur navigue sur internet; il tape des adresses dans son navigateur.
Ces adresses sont appelées des URL.

Exemples : 
- https://www.ind-philippeville.be/ 
- https://www.python.org/about/gettingstarted/
- https://www.facebook.com/<nom_utilisateur>
- https://www.wikifin.be/fr/recherche?sépargne


#def une URL (Uniform Resource Locator) est une chaîne de caractères **unique** qui sert à localiser une ressource (page web, fichier, image,...) sur internet.
![[url_anatomy.png]]


#def les *routes* établissent un *mapping* (ou correspondance), entre une **URL** et une **vue**; c'est à dire entre une adresse internet et la fonction (python, dans notre cas) qui va renvoyer la ressource (page web, fichier, image,...).

## Les vues

#def les *'vues'* sont des fonctions Python qui reçoivent une requête web et renvoient une réponse.

Ce sont elles qui feront tout le traitement et renverront au client (le navigateur) le contenu à afficher sur la page web.

**Rem** : nous traduirons ici le terme anglais *view function* par le terme français *fonction de vue*.
- On aurait également plus le traduire par *fonctions de contrôle*... 
- Dans le cadre de ce cours, par raccourci, on parlera également de *vues* pour désigner les *fonctions de vue*.

## Les routes : url <-> vue  
Comme expliqué ci-dessus, les *routes* permettent de faire correspondre une **URL** à une **vue**.
Concrètement : 
- lorsque l'on introduira une adresse internet (URL) dans notre navigateur; 
- celle-ci sera redirigée vers notre serveur web en Flask (ici dénommé : 'app')
- le serveur web, analysera le chemin décrit dans l'URL
- et appelera la fonction Python correspondante que nous aurons créée pour y répondre.

```python
@app.route('/')
def index():
    return "<h1>Bonjour la classe !</h1>"
```

La correspondance entre l'URL et la fonction qui lui répond est établie à l'aide de `@app.route()`
- `@` : permet de définir en Python, ce que l'on appelle un *décorateur* (c'est une manière d'ajouter de nouveaux comportements à un code existant sans modifier ce dernier... mais nous ne nous étendrons pas davantage sur le sujet).
- `app` : c'est le nom que nous avons donné à notre application Flask (cf. ci-dessus)
- `.route()` : est une méthode associée à notre application *'app'* ; et qui permet d'associer
	- <u>le chemin</u> décrit par la partie de l'URL indiqué entre les parenthèses.
	  **Rem** : le chemin passé comme argument est une chaîne de caractères; et est donc placé entre des guillemets.
	- <u>à la fonction</u> qui est définie juste en-dessous

# Démarrage du serveur web
```python
if __name__ == '__main__':
    app.run(debug=True)
```


# La séquence complète 
Regroupons à présent le code décrit ci-dessus dans un fichier que nous nommerons `app.py`

**Rem** : ce nom est arbitraire, nous aurions pu l'appeler autrement; mais vu que c'est le nom que nous avons donné à notre application web; c'est celui que nous utiliserons par la suite.

**app.py**
```python
# Création de l'application web
from flask import Flask
app = Flask(__name__)

# Notre première route
@app.route('/')
def index():
    return "<h1>Bonjour la classe !</h1>"

# Démarrage de notre application web
if __name__ == '__main__':
    app.run(debug=True)
```

Exécutez ce fichier à l'aide de votre interpréteur Python : `python app.py`
**Rem** : n'oubliez pas que Flask est installé ici dans un environnement virtuel.  C'est donc l'interpréteur Python de cet environnement virtuel (venv) que vous devez invoquer.
![[run_flask_app.png]]

# A quelle adresse joindre votre serveur web ?
Par défaut, une fois démarré, notre serveur web écoute :
- A l'adresse : http://127.0.0.1  (également appelée `localhost`)
- Sur le port : 5000
Pour joindre notre serveur, on tapera donc http://127.0.0.1:5000 


<div style="font-size:100px;display:flex;justify-content:center "> 🙂</div>
