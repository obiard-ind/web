# Envoi de requêtes GET depuis le navigateur
Les *requêtes HTTP* `GET` sont utilisées pour demander une ressource au serveur.

**Limitations**
- Sur la longeur : maximum 2048 caractères
- Sur le type de données : seuls les caractères ASCII sont autorisés.

**Transmission**
Ce type de requête peut être adressée au serveur :
- soit directement depuis l'URL
- soit via un formulaire utilisant la méthode `get`
## Depuis l'URL
Une requête `GET` s'exprime sour la forme d'une chaîne de caractères passée directement depuis l'[[1-Application basique#Les routes|URL]].  
- Elle commence par un `?` situé juste après le *chemin* dans l'URL 
- Elle est constituée de paires `nom=valeur` séparées par le caractère `&`

*Exemples de requêtes GET depuis une URL*
```
https://www.example.com:443/blog/article/search?lang=fr&docid=775
https://localhost:5000/taches/list?username=doe&start=20250101&end=20251231
```
## Depuis un formulaire
Une requête `GET` peut également être envoyée au serveur depuis un formulaire utilisant la méthode `get` !

*Exemple de requête GET envoyée au serveur depuis un formulaire*
```html
<form action="/task/list" method="GET">
    <label for="username">Entrez votre nom : </label>
    <input type="text" id="username" name="username"><br>
    <input type="submit" value="SEND">
</form>
```

# Capture de requêtes GET par les serveur
En Flask, les requêtes GET seront capturées par le serveur au niveau de la [[1-Application basique#Les routes url <-> vue|route]] !

*Exemple de route capturant une requête GET*
```python
@app.route('/task/list', methods=['GET'])
def liste_taches():
    username =  request.args.get("username")
    if username:
        return f"Bonjour {username}"
    else:
        return render_template('afficher_formulaire.html')
```

On notera dans cet exemple :
- l'utilisation d'un nouveau paramètre `methods` dans la définition de la route.
  Celui-ci prend en argument une liste indiquant quels types de requêtes : `['GET']`,`['POST']`,`['GET','POST']` sont acceptées par cette route.  Ici, seules les requêtes `GET` le sont.
- comment nous pouvons récupérer la valeur d'un argument transmis via une requête `GET`.
  Ceci se fait à l'aide de la méthode `requet.args.get("nom_argument")`, ou *nom_argument* doit correspondre au nom de l'argument transmis via l'URL ou le formulaire.



