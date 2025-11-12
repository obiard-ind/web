# Architecture 'client-serveur' 
Quand vous naviguez sur le web; vous le faites au départ d'un appareil (PC, Tablette, Smartphone,...), et accédez à des ressources hébergées sur un serveur quelque part sur internet.

#def Le **client** : c'est ce qui demande un service, accède à une ressource
#def Le **serveur** : c'est ce qui fournit ce service, fournit la ressource.

Pour qualifier une architecture de communication entre un client et un serveur, on parlera d'**architecture client-serveur** !

A l'instar de nous, les humains; les ordinateurs ont besoin de règles et de mots pour communiquer.
Par exemple : nous utilisons le français; et lorsque nous parlons, l'usage (et la politesse) est que tandis que l'un parle, l'autre écoute; puis lui répond, etc...

#def Un **protocole** est l'ensemble de ces règles et procédures qui définissent la manière que 2 entités (ordinateurs, humains,...) vont utiliser communiquer entre elles.
# Le protocole HTTP
Pour les communications sur le web; le protocole le plus utilisé est **le protocole HTTP** !

Le protocole HTTP a été développé au [CERN](https://home.web.cern.ch/fr) par [Tim Berners-Lee](https://fr.wikipedia.org/wiki/Tim_Berners-Lee) à la fin des années '80, et annoncé publiquement en 1991 sous le nom de projet `WWW` ! (ça ne vous dit rien ? <span> &#x1f609;</span>  )

## HTTP Request / Response
La communication entre un *client* et un *serveur* s'effectue au travers de l'échange de messages.
Cet échange s'effectue selon les règles du *protocole HTTP*.

Ces messages contiennent diverses <u>informations structurées</u> que nous allons détailler ci-après.
Il peuvent être de deux types :
#def **HTTP request** : la demande que le client adresse au serveur
#def **HTTP response** : la réponse du serveur au client


![[client-server-communication.webp]]

**Métaphore** : *un client passe une commande au serveur dans un restaurant*
```
Le client contacte le serveur; le serveur, quand il est disponible, répond au client.
Le client passe sa commande au serveur sous forme d'information structurée : plat, quantité, type de cuisson,...
Le serveur traite la commande (enfin, c'est la cuisine qui fait le boulot; mais c'est 'transparent' pour le client); et répond au client en lui délivrant les plats correspondants à sa commande.
```

### HTTP request
Une *requête HTTP* contient les informations structurées suivantes :
- Une __*méthode*__ : indique l'action à effectuer
	- `GET` : demander une ressource au serveur.
	- `POST` : envoyer une information au serveur pour créer ou mettre à jour une ressource en utilisant l'information contenue dans le **corps** de la requête.
	- ... mais il y en d'autres : `PUT`, `DELETE`, ...
- Une __*URI*__ : indique la ressource demandée (page html, fichier css, image,...)
- Des __*en-têtes*__ (headers) : qui contiennent des méta-données sur la requête (adresse IP, type de navigateur,...)
- Un __*corps*__ (body) : pouvant contenir des données par exemple, lors d'une requêtes de type `POST` (par exemple : envoi d'informations depuis un formulaire)

### HTTP response
Une *réponse HTTP* contient les informations structurées suivantes :
- Un [__*code de statut*__](https://www.w3schools.com/tags/ref_httpmessages.asp) (status code) : indique le résultat du traitement de la requête
	- `1xx` : Information
	- `2xx` : Succès
		- `200 OK` : la requête a été satisfaite / tout s'est bien passé !
		- `3xx` : Redirection
	- `4xx` : Erreur côté *client*
		- `403 Forbidden` : la requête est valide; mais refusée par le serveur
		- `404 Not Found` : la ressource demandée par le client n'a pu être trouvée !
	- `5xx` : Erreur côté *serveur*
		- `503 Service unavailable` : le serveur est indisponible (arrêté ou surchargé)
- Des __*en-têtes*__ (headers) : qui contiennent des méta-données sur la réponse
- Un __*corps*__ (body) : qui contient le contenu demandé, la ressource (eg. le code html d'une page web, une image, un fichier javascript ou css,...)






