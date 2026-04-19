**Bootstrap** est un framework CSS permettant de développer rapidement des interfaces web qui s'adaptent à la taille de l'écran (PC / Smartphone / Tablette)

**Bootstrap** présente plusieurs avantages :
- Rapidité de mise en oeuvre (plus besoin de créer ses propre classes)
- Uniformisation du site avec un look 'professionnel'
- Garantir la compatibilité de l'affichage entre les navigateurs.
- Contenu adaptable à la taille de l'écran du client
- etc...

Concrètement, il s'agit d'un ensemble de **classes CSS** utilisables directement dans vos projets.
Des scripts Javascript sont également proposés afin de dynamiser certains composantes (eg. menu déroulants,...)

**Note** : l'utilisation de Bootstrap n'exclu pas l'utilisation de feuilles de style personnalisées.  Il suffira alors de les importer après la feuille de style Bootstrap (la dernière feuille téléchargée ayant la plus haute priorité en cas de conflit de nom).

# Documentation
Le téléchargement, la documentation, les exemples (et le code directement utilisable) sont disponibles directement sur le site https://getbootstrap.com 
## Importation
Pour pouvoir utiliser Bootstrap; il suffit juste d'importer la feuille de style; et éventuellement, le fichier Javascript dans les pages HTML des templates que vous renvoyez au client.
#### Soit depuis le serveur local :
```html
 <link rel="stylesheet" 
       href="{{ url_for('static',filename='css/bootstrap.min.css') }}">
 <script src="{{ url_for('static',filename='js/bootstrap.bundle.min.js') }}" 
         defer></script>
```
- Vous reconnaîtrez ici la manière dont Jinja, le moteur de templates de Flask accède aux ressources.
- Requiert que les fichiers référencés soient stockés sur le serveur local, à l'emplacement indiqué.
	- Ref : [Localisation des fichiers dans l'arborescence de Flask](2-Templates%20-%20Intro.md#Localisation%20dans%20l'arborescence)
	- Téléchargement depuis le site de[Bootstrap](https://getbootstrap.com/docs/5.3/getting-started/download/) > Choisir "Compiled CSS and JS" > Télécharger le fichier '.zip' > Extraire les fichiers, et copier à la destination les deux fichiers nommés : `bootstrap.min.css` et `boostrap.min.js`
- Comme les fichiers sont disponibles localement; peut fonctionner en l'absence de connexion internet.
#### Soit via CDN :
```html
<link href="https://cdn.jsdelivr.net/npm/bootstrap@5.3.8/dist/css/bootstrap.min.css" rel="stylesheet" integrity="sha384-sRIl4kxILFvY47J16cr9ZwB07vP4J8+LH7qKQnuqkuIAvNWLzeN8tE5YBujZqJLB" crossorigin="anonymous">
<script src="https://cdn.jsdelivr.net/npm/bootstrap@5.3.8/dist/js/bootstrap.bundle.min.js" integrity="sha384-FKyoEForCGlyvwx9Hj09JcYn3nv7wiPVlz7YYwJrWVcXK/BmnVDxM+D2scQbITxI" crossorigin="anonymous"></script>
```
- Requiert juste que le client qui reçoit la page html aie accès à internet pour pouvoir télécharger la feuille se style et le fichier Javascript.
- **Tip** : c'est souvent la solution privilégiée (et la plus simple) lorsque le client a accès à internet.

