# Envoi de requêtes `POST` depuis le navigateur
Les *requêtes HTTP* `POST` sont utilisées pour **transmettre une ressource** au serveur.

**Limitations**
- Effet de bord : risque de double transmission en cas de rafraîchissement de la page (Solution : [[4b-Requete POST#Pourquoi une redirection ?|pattern PRG]])
**Avantages**
- Sur la taille : quasi pas de limitation !
- Sur le type de données : texte, binaire (pdf, video,...)
- Possède un `body` : les requêtes `POST` n'affichent pas leur contenu dans l'URL
- Sécurité : encryption possible via HTTPS.
- Historique / bookmarks : non

**Transmission**
Ce type de requête peut être adressée au serveur :
- via un formulaire utilisant la méthode `post`
## Depuis un formulaire
Une requête `POST`  est envoyée au serveur depuis un formulaire utilisant la méthode `POST` !

*Exemple de requête POST envoyée au serveur depuis un formulaire*
```html
<form action="/task/add" method="POST">
    <label for="nom">Entrez votre nom : </label>
    <input type="text" id="nom" name="nom"><br>
    <label for="prenom">Entrez votre prénom : </label>
    <input type="text" id="prenom" name="prenom"><br>
    <input type="submit" value="SEND">
</form>
```

# Capture de requêtes `POST` par le serveur
En Flask, les requêtes POST seront capturées par le serveur au niveau de la [[1-Application basique#Les routes url <-> vue|route]] !

*Exemple de route capturant une requête POST*
```python
@app.route('/task/add', methods=['GET','POST'])
def ajouter_tache():
	if request.method == 'POST':
	    nom = request.form.get("nom")
	    prenom = request.form.get("prenom")
	    # Traitement des données
	    return redirect(url_for("success"))
    return render_template('formulaire_ajouter.html')

@app.route("/success")
def success():
    return "Formulaire transmis avec succès !"
```

On notera dans cet exemple :
- l'utilisation du paramètre `methods` dans la définition de la route; prenant en argument la liste des méthodes autorisées; à savoir `['GET','POST']` !
  Les deux méthodes doivent être spécifiées.  En effet : 
	- Lorsque l'utilisateur accédera à la route la première fois; ce sera une requête `GET` qui sera envoyée; ce qui aura pour conséquence de transmettre la page avec le formulaire à afficher.
	- Lorsque l'utilisateur validera le formulaire en utilisant la méthode `POST`; la condition `request.method == 'POST` sera vérifiée; et les données du formulaire seront traitées.  Une redirection s'opérera en vers l'URL correspondant au *point de terminaison* `"success"`.
- La récupération de la valeur des champs d'un formulaire transmis via une requête `POST`.
  Ceci se fait à l'aide de la méthode `request.form.get("nom_champ")`, ou *nom_champ* doit correspondre au nom du champ correspondant du formulaire.
## Pourquoi une redirection ? 
Lorsque le *contrôleur* renvoie une réponse au *client* sous forme d'une chaîne de caractères (eg. `return f"Formulaire bien reçu"`, ou via `render_template()`); ceci a pour effet de **rafraîchir la page** et d'afficher le nouveau contenu.

Or, lorsque la dernière requête effectuée par une page est une requête `POST`, le rafraîchissement de la page va déclencher une nouvelle transmission de la requête par le navigateur.
Ceci peut avoir des conséquences indésirables : transmission en double; message d'avertissement à l'intention de l'utilisateur...

La solution consiste alors :
- à ne pas renvoyer un contenu vers la page
- mais à renvoyer une redirection (à l'aide de la fonction `redirect()`).
  En effet, une redirection ne renvoie pas de chaîne de caractère; mais une URL.
  Or, l'accès à une URL déclence une requête `GET` pour obtenir une réponse; et ne va dès lors pas rafraîchir la page; et ainsi éviter une double transmission des données du formulaire.

Ceci s'appelle le **pattern PRG** : Post-Redirect-Get !

