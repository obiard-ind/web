A l'étape précédente, nous avons vu comment :
- utiliser des parties du chemin de l'URL comme variables
- passer des variables au fichier template en appelant la fonction [[2-Templates - Intro#La fonction *render_template()*|render_template()]] avec des paramètres additionnels.
- utiliser ces variables dans le fichier template, au sein d'expressions délimitées par `{{ }}`

Dans cette partie; nous verrons qu'il est également possible d'utiliser ces variables au sein de structure de contrôle (boucles, structures conditionnelles) afin de réaliser des traitements au sein même du **template** avant de renvoyer le document au navigateur pour affichage.

# > Les structures de contrôle : **{% ... %}**
Les structures de contrôle seront délimitées par les symboles `{%  %}`

## Structure conditionnelle : 
Dans un *template* Jinja, les structures conditionnelles s'écriront avec la syntaxe suivante :
```
{% if expression_conditionnelle %}
    {# contenu à renvoyer si l'expression est évalue à True #}
{% else %}
    {# contenu à renvoyer si l'expression est évaluée à False #}
{% endif %}
```
**Rem** : les commentaires, en Jinja, s'écrivent entre les balises `{# ... #}`

### Exemple :
Reprenons l'exemple vu précédemment; lequel affichait le résultat de l'élève dont le nom était transmis via l'URL.
```python
@app.route('/notes/<eleve>)
def note_eleve(eleve):
	points = {'Sylvie':18,'Alain':14,'Auguste':9,'Bernard':11,'Gilbert':13}
	return render_template('resultats.html',eleve = eleve, points = points)
```

... dans notre précédent template; voici ce qui s'affichait lorsque l'on que l'on passait via l'URL le prénom d'un élève qui n'apparaissait pas dans notre dictionnaire de points ? Par exemple :
`https://localhost:5000/notes/Isabelle`

```
L'élève Isabelle a obtenue None points.
```

... et de résoudre ce problème à l'aide de la structure conditionnelle dans le template

*resultats.html*
```Jinja
{% if eleve in points %}
    L'élève {{ eleve }} a obtenu {{ points.get(eleve) }} points.
{% else %}
    L'élève {{ eleve }} n'a pas de points attribués à son nom.
{% endif %}
```

## Boucle 'for' :
Dans un *template* Jinja, les boucles 'for' sont exprimées à l'aide de la syntaxe suivante :
```
{% for element in elements %}
    Traitement de l'élément : {{ element }}
{% endfor %}
```
**Rem** : on notera que la variable `element` pourra être utilisée au sein de la boucle; à l'interieur d'expressions; c'est à dire, entre les accolades doubles **{{ .. }}** que nous avons vues précédemment.

### Exemple :

```python
capitales = {
    "Belgique": "Bruxelles",
    "France": "Paris",
    "Danemark": "Copenhague",
    "Irlande": "Dublin",
    "Grèce": "Athènes",
    "Turquie": "Ankara",
    "Inde": "New Delhi",
    "Pakistan": "Islamabad",
    "Chine": "Beijing",
    "Australie": "Canberra"
    }

@app.route('/capitales)
def list_capitales():
	return render_template('capitales.html',capitales = capitales)
```
**Rem** : `capitales` est déclarée en dehors de toute fonction.  Elle est donc considérée comme **globale** par rapport à celles-ci, et accessible par son nom au sein de ces dernières.

*capitales.html*
```Jinja
<h1>Liste des capitales par pays : </h1>
<table>
	{% for pays, capitale in capitales.items() %}
	    <tr>
		    <td>{{ pays }}</td>
		    <td>{{ capitale }}</td>
	    </tr>
	{% endfor %}
</table>
```

