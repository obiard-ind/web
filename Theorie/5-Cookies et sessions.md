# Motivation 

Le protocole HTTP est *stateless*; c'est à dire que chaque requête entre le *client* et le *serveur* est considérée comme une transaction indépendante des autres.  Il n'y a donc pas de partage d'information entre des requêtes successives.

**Métaphore** : *un client passe une commande au serveur dans un restaurant*
```
Pour revenir à la métaphore du client qui passe commande auprès d'un serveur dans un restaurant; c'est un peu comme si, à chaque fois que le client contacte le serveur, ce dernier avait tout oublié et du client, et de la commande qu'il a passée...  C'est assez gênant !
```

Dans le cas de requêtes HTTP successives; cela signifierait qu'à chaque fois que l'utilisateur aurait besoin d'accéder à une ressource qui nécessite d'être authentifié; il faudrait recommencer l'authentification; ou qu'il serait impossible de conserver les paramètres de préférence de l'utilisateur (eg. thème ou langue du site) entre 2 requêtes.

# Solution
Il existe plusieurs solutions au problème de conservation de l'information entre les requêtes.
Nous n'en aborderons qu'une seule ici : l'utilisation des **cookies**
## En théorie : les cookies
#def un **cookie** est un petit bloc d'information, créé et envoyé par le serveur au navigateur; pour y être stocké sous la forme d'un fichier <u>sur le PC du client</u>.
### Principe de fonctionnement
Chaque fois que le navigateur se connectera au site qui l'a créé; le navigateur vérifiera l'existence des cookies pour ce site; et renverra l'information stockée dans ceux-ci au serveur.
Rem : un serveur peut en effet stocker plusieurs cookies sur le PC du client.

**Métaphore** : *le carnet de commandes du serveur*
```
Pour revenir à la métaphore du 'serveur' amnésique; c'est un peu comme si, pour résoudre son problème de mémoire; celui-ci se trouvait désormais doté d'un petit carnet de commande; ou il enregistre les commandes passées par les différentes tables.
Désormais, à chaque fois qu'un client à une table l'appelle; il ouvre son carnet à la page correspondante; et relis les informations qu'il aura enregistrées pour cette table.
```
### Quelles informations ?
Un cookie peut stocker toutes sortes d'informations; et plusieurs cookies peuvent être créés pour un même site.

Les cookies sont souvent utilisés pour conserver des informations de connexion : telles que le nom du compte qui a servi à l'utilisateur pour se logger.
Ils servent également à enregistrer des préférences de l'utilisateur; telles que la langue souhaitée ou le thème pour personnaliser l'affichage du site.
D'autres informations; telles qu'un panier d'achat peuvent également être stockés sous la forme de cookie.
### Et la sécurité dans tout ça ?
Les cookies étant stockés sous forme de fichiers sur le PC de l'utilisateur; il est possible d'en consulter le contenu.  Ceci devient particulièrement sensible lorsque plusieurs utilisateurs se partagent le PC (ceci étant aggravé lorsqu'ils utilisent un même compte); comme sur des PC publics, par exemple !

En outre, comme le contenu du cookie est transféré à travers le réseau; il est possible que son contenu puisse être intercepté, lu, voire être altéré !

Les principaux risques sont la confidentialités de données; mais aussi la sécurité de l'accès au site, si l'attaquant pouvait réutiliser ces données pour y accéder à la place de l'utilisateur légitime.
#### Confidentialité et intégrité
La confidentialité et l'intégrité sont deux concepts importants en matière de sécurité informatique.
#def la **confidentialité** est le principe selon lequel l'information ne doit être accessible qu'aux personnes autorisées.
#def l'**intégrité** est le principe permettant de garantir que l'information n'a pas été altérée !

La sécurité informatique est un domaine très vaste que nous n'aborderons pas ici !
Elle implique :
- Des techniques informatiques : notamment, le **cryptage** et la **signature**
- Des *bonnes pratiques* : 
	- **Attn** : ne jamais mettre d'informations sensibles (données personnelles, mots de passe,...) dans des cookies !
	- **Attn** : ne jamais partager les clés de chiffrement pour le cryptage et la signature (par exemple, en les exposant sur GitHub...)
	- etc...
- etc...
#### Cryptage et signature
#def Le **chiffrement** (ou **encryption** en anglais) : permet, à l'aide d'une **clé de chiffrement** (sous la forme d'une suite de caractères aléatoires) et d'un **algorithme de chiffrement**; de transformer un texte clair (lisible), en texte chiffré (illisible).

Sans la connaissance de la clé; impossible de *déchiffrer* le texte; c'est à dire, de rendre le texte chiffré en texte clair à nouveau.

Le <u>chiffrement</u> est donc la technique utilisée pour garantir la <u>confidentialité</u> des données.

#def La **signature** : permet de garantir que le contenu n'a pas été altéré (et/ou l'identité de l'expéditeur).  Elle repose aussi sur des techniques cryptographiques que nous ne verrons pas ici.

La <u>signature</u> est donc la technique utilisée pour garantir l'<u>intégrité</u> des données.

## En pratique : l'objet `session`

L'objet `session` de Flask permet d'enregistrer des données sous la forme d'un `cookie` qui est alors transmis au navigateur.
```python
from flask import session
```

### Sécurité de l'objet `session`
L'objet `session` **signe** le cookie; mais ne le **chiffre pas** !
C'est à dire que les informations stockées dans le cookie sont lisibles *en clair*; mais qui si le cookie est modifié; alors le serveur le détectera et n'autorisera pas que ses données soient à nouveau utilisées par le serveur !

Afin de permettre à l'objet `session` de *signer* le cookie; il faudra définir une **clé secrète** au niveau de notre application. 
```python
# Pour générer la clé : python3 -c "import secrets; print(secrets.token_hex(24))"
app.config['SECRET_KEY'] = "6bac0f8ede9bae0fdf22be2b39650369d15e4af1ffeec138"
```
Où :
- `app` : désigne notre application web
- `.config['SECRET_KEY']` est l'entrée correspondant à la clé secrète dans le dictionnaire des paramètres de configuration de notre application.

### S'utilise comme un dictionnaire
L'objet `session` s'utilise comme un *dictionnaire* !
#### Pour ajouter / modifier une information

```python
session['nom_utilisateur'] = nom_utilisateur
```
#### Pour récupérer une information
```python
# Retourne la valeur associé à la clé; ou None si la clé n'existe pas !
session.get('nom_utilisateur') 
```
#### Pour supprimer une information
```python
# Supprime la clé 'nom_utilisateur' et la valeur associée
# Rem : renvoie None si la clé n'existe pas !
session.pop('nom_utilisateur',None)
```

#### Exemple :
Si un traitement requiert que l'utilisateur soit loggé; on peut utiliser le bout de code suivant qui renverra vers la page de 'login' si l'objet `session` ne comporte pas d'entrée pour la clé 'nom_utilisateur'; et exécutera le code après le `if`, dans le cas contraire !

```python
# L'utilisateur doit être loggé !
if not session.get('nom_utilisateur'):
    return redirect(url_for('login'))
```




