# ZZ Book

Application microservice pour le cours de Technologie des conteneurs de ZZ3.

_Auteurs_ : Stacy VARLOTEAUX, Justine BACHELARD et Axel REMACK

## Code des services
Le code des différents services est présent dans `src/`.

* `details` : Service de détails des livres, en ruby.
* `productpage` : Service point d'entrée des autres microservices, en python.
* `reviews` : Service contenant les commentaires sur les livres, appel `ratings`, en java.
* `ratings` : Service de gestion des notes des livres, en node.js.

## Définitions de l'API
L'API visible de l'utilisateur est définie dans le fichier `swagger.yaml`. Pour visualiser le fichier, vous pouvez utiliser le [Swagger editor](https://editor.swagger.io/).

## Fichiers de conteneurisation et d'orchestration
### Conteneurisation
Chaque service possède un `DockerFile`, présent dans son dossier.

A la racine du projet est présent le fichier `docker-compose.yml`.

### Orchestration
Les manifests des services sont disponibles dans le dossier `manifest/` à la racine du projet.

## Informations complémentaires
Pour toute information complémentaire (étapes de travail, commandes utilisées, solution utilisée pour l'hébergement des images, ...), veuillez vous référer au document `CR.md` à la racine du projet. Ce document présente simplement le déroulé des TPs, sans travail excessif sur la forme.

Enjoy :)


