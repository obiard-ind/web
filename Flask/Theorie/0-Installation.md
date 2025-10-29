# Environnements virtuels

Chaque environnement virtuel :
- possède une copie de l'interpréteur principal
- possède ses propres packages.
Avantages :
- pas de risque de conflits de version
- pas besoin de droits administrateurs pour l'installer et l'exécuter
## Installation
```windows
$ python -m venv <nom_du_projet>   # dans nos exercices : <nom_du_projet> = venv
```
## Activation
```windows
$ venv/scripts/activate
```
**Rem** : lorsque l'environnement virtuel est activé, la mension (venv) apparaîtra en début de la ligne de commande :
![[venv_activate.png]]
## Désactivation
```windows
$ venv/scripts/activate  # ou plus simplement : deactivate
```

# Flask

## Qu'est-ce que Flask ?
Flask est un micro-framework web écrit en Python.

#def un **framework** (ou cadre de travail) est une structure logicielle offrant un ensemble de composants réutilisables, d'outils et de normes, qui sert de squelette pour développer de nouvelles applications. 

## Pourquoi Flask ?
Il existe de nombreux frameworks pour le développement d'applications web !
Notre choix s'est porté sur Flask pour les raisons suivantes :
- Il est petit : < 10Mo.
- Il est modulaire : de nouvelles fonctionnalités peuvent être aisément ajoutées; on ne choisit que ce que l'on utilise.
- Il est flexible : on est moins contraint qu'avec certains autres frameworks.
- Il est facile à installer
- Il est écrit en Python : ce qui permettra de réutiliser vos connaissances déjà acquises dans ce langage.
- Il est rapide : surtout pour les petites applications, microservices et APIs
- Il est facile d'apprentissage.

Quelques sites qui utilisent Flask (au moins pour une partie de leurs sites) : 
- Reddit (https://www.reddit.com) 
- Netflix  (https://www.netflix.com)
- Pinterest (https://www.pinterest.com)
- ...


## Installation de Flask
```
(venv) $ pip install flask
```
## Test de l'installation
```
(venv) $ python
>>> import flask
>>> 
```
S'il n'y a pas de message d'erreur, c'est que tout s'est bien passé !

