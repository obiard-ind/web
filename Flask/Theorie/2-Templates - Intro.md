#def Un *template* est un fichier décrivant le contenu à afficher, en réponse à une requête.

Les templates sont constitués :
- De contenu statique (du texte, du code HTML,...)
- De contenu dynamique (des expressions et des instructions)

# Localisation dans l'arborescence
Par défaut, les *templates* sont situés dans un sous répertoire `/templates` de l'application.
```
/web/mon_app/
|
|-- app.py                # point d'entrée de l'application
|-- requirements.txt      # liste de dépendances Python
|-- templates/            # templates (index.html,...)
|   |-- index.html
|-- static/               # ressources 'statiques' (css, js, images,..)
|   |-- css/              # fichiers css 
|       |-- style.css 
|   |-- js/               # fichiers javascript
|       |-- scripts.js
|   |-- images/           # fichier d'images
|       |-- logo.png      
|-- venv/                 # environnement virtuel Python (interpréteur, modules,..)
```
#best_practice faire correspondre le nom des répertoires (dans *templates*) à ceux des routes qui les renvoient :
- Eg : `@app.route('/software/opensource)` => `/templates/software/opensource.html`
# La fonction *render_template()*
La fonction `render_template()`, prend en argument un document *template*; dont elle remplacera les variables par leur valeur, pour retourner un document html. 

## Importation de la fonction
```python
from flask import render_template
```

## Syntaxe basique

**Syntaxe** : `render_template('nom_du_template.html')`
Fonctionnement :
- récupère le *template* (i.e : le fichier) passé en argument
- remplace les variable qu'il contient par leur valeur
- retourne le document html résultant.

## Utilisation dans les *routes*
Au lieu de renvoyer une simple chaîne de texte; comme nous l'avons fait lors de la création de notre [[1-Application basique|Application basique]]; nous pouvons désormais renvoyer un document html complet.

```python
@app.route('/')
def index():
    return render_template('index.html')
```


