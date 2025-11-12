Une page web typique contient des liens vers d'autres pages et / ou ressources pour s'afficher correctement.
Vous les avez déjà rencontrés; par exemple, dans les balises HTML suivantes :
- `<img src="mon_logo.jpg" alt="Logo de l'école" width="50" height=50">`
- `<a href="home.html">Accueil</a>`
# Générer des URL dynamiquement : url_for

## Prérequis : importer `url_for`
Avant de pouvoir générer des URL dynamiquement à l'aide de la fonction `url_for`; il faudra d'abord importer celle-ci dans votre projet.

L'importation de la fonction `url_for` se fait dans des fichiers Python.

Typiquement, on l'importera dans les fichiers suivants :
#### Dans les fichiers où sont définies les *routes*
```python
from flask import url_for

# Définition des routes
@app.route('/home')
def home()
    # code spécifique à cette route...

```
#### Dans les fichiers où nous définirons nos *formulaires*
**Rem** : nous aborderons la création de formulaires en Flask ultérieurement.
#todo Compléter cette section une fois que nous aurons vu les formulaires.

## Utilisation : où, pourquoi, comment ?
Nous utiliserons la génération dynamique d'URL à l'aide de la fonction `url_for` dans les cas suivants :
### Accéder à une page interne du site
#### Problème
Imaginez votre site comportant plusieurs dizaines, voire centaine de pages web; et que celles-ci contiennent des liens renvoyant vers d'autres pages du site (par exemple : la page d'accueil, ou la page de contact).
Si vous venez à changer le nom de cette page; ou déplacer celle-ci dans l'arborescence de votre site; il faudra alors trouver et adapter tous les liens, dans toutes les pages qui y font référence.
Ceci est non seulement fastidieux; mais aussi source d'erreur et d'oubli... conduisant bien souvent à des erreurs de type : **404 - Not Found**
#### Solution
Flask propose une solution : la fonction `url_for()`
**Explication** : comme en Flask, la page est renvoyée depuis une fonction Python associée à une route; il suffira de passer en argument le nom de cette fonction (également appelée *point de terminaison* ou *endpoint*, en anglais) à `url_for()`.
Ainsi, si le nom ou la localisation de notre fichier *template* vient à à changer; il suffira de l'adapter un seul endroit; c'est à dire dans la fonction correspondant au point de terminaison !

*Dans le fichier de défintion des routes*
```python
@app.route("/home")
def home():     # 'point de terminaison', ou 'endpoint'
    return render_template("home.html")
```

*Dans un template quelconque qui contient un lien vers la page d'accueil*
```html
Pour retourner à l'accueil, cliquez : <a href="{{ url_for('home') }}">ici !</a>
```

**Rem** : notez bien les accolades `{{ }}` autour de la fonction; car il s'agit d'une [[2a-Templates - expressions|expression]]
**Rem** : notez également que l'argument qui correspond au point de terminaison est le **nom de la fonction**, <u>sans</u> les parenthèses !

### Accéder à une ressource 'statique'
#def Les ressources qualifiées de statiques, sont des fichiers dont le contenu ne change pas dynamiquement au chargement de la page; et qui sont envoyés tels quels au navigateur.

En web, il s'agit typiquement :
- d'images
- de feuilles de style (CSS)
- de fichiers Javascript
- etc ...

En Flask, on placera ces fichiers dans le sous-répertoire `static` de [[2-Templates - Intro#Localisation dans l'arborescence|l'arborescence de notre application]].

*Dans un fichier template*
```html
<img src="{{ url_for('static', filename='images/logo.png')" alt="Logo" }}
```

**Rem** :
- notez les **guillemets simples** (apostrophes) autour des mots `'static'` et `'images\logo.png'` pour éviter la confusion avec les guillemets doubles qui entourent l'url passée à `src` !
- notez également que le chemin vers le fichier est un **chemin relatif** exprimé au départ du répertoire `static` !









