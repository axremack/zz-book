
# TP1

## Partie I
Conteneurd permet d'éxécuter les conteneurs

**Q1** - Installez docker sur votre machine : [https://docs.docker.com/install/](https://docs.docker.com/install/) 
Validez l’installation en exécutant la commande docker ps

https://docs.docker.com/engine/install/debian/
https://www.kali.org/docs/containers/installing-docker-on-kali/

Problèmes de permissions
Soit on fait tout avec sudo soit :
```
sudo groupadd docker
sudo usermod -aG docker aremack
```
Puis on redémarre


**Q2** - Récupérez l’image officielle “alpine” sur votre machine,  quelle commande utilisez-vous ? Listez les images afin de vérifier qu’alpine est bien présente.
```
docker pull alpine
docker images
```
**Q3** - Démarrez un conteneur alpine en mode interactif. Vérifiez que vous pouvez utiliser les commandes standards. Quittez votre conteneur (exit), que se passe t’il si vous faites docker ps ?

`docker ps -a` -> On a la liste des conteneurs (-a pour avoir les stopped)
`docker run -it c059bfaa849c`
On met l'id du conteneur pour le lancer

Avec docker ps, on voit qu'il n'y a plus de conteneurs en mode "actifs" si on sort du mode interactif --> il se stop dès qu'on sort


**Q4** - Démarrez un conteneur mariadb, appelez le “mariadbtest”. Que faut-il ajouter à la ligne de commande Docker pour démarrer la base correctement ? Utilisez docker ps pour vérifier que votre conteneur est en cours d'exécution.

`docker run --name mariadbtest -e MARIADB_ROOT_PASSWORD=test mariadb`

En vérifiant avec docker ps, le conteneur est bien lancé


**Q5** - Quelle commande utiliser pour obtenir un shell dans le conteneur “mariadbtest” ?

Une fois le conteneur lancé, on récupère son identifiant en l'affichant (docker ps) puis on fait la commande suivante :
`docker exec -it 4bb942558750 bash`


**Q6** - Exécutez la commande “$ docker top mariadbtest”, comparez le résultat avec la commande ps de votre machine (ex. “$ ps -aux”). Que constatez-vous ?

La commande docker top présente beaucoup moins d'entrées.
--> le conteneur docker n'a accès qu'à ses propres processus


**Q7** - Démarrez un conteneur, en respectant les spécifications suivantes :
-   Image : dockersamples/static-site
-   Nom du conteneur : my-site
-   Variables d'environnement : AUTHOR="You"
-   Le port 80 du conteneur doit être accessible sur votre machine (au port de votre choix).
    
`docker pull dockersamples/static-site`

`docker run --name my-site -e AUTHOR="Me" -p 8080:80 dockersamples/static-site`
Pas --expose car cette instruction est simplement déclarative
-p prends port sur l'hote : port sur le conteneur

On va ensuite sur localhost:8080 sur la VM et on voit bien une page apparaitre



## Partie II 
**Q1** - Initialiser un dépôt Git sur votre machine et le pusher sur Github/Gitlab. 
Fait par Justine -> https://github.com/LimseDanlove/docker-axel-stacy-justine
  

**Q2** - Récupérez les sources des microservices (fichier zz-book.zip) et décompressez-les dans le dossier de votre projet. Éditez le fichier README.md en ajoutant la liste des membres de votre équipe ainsi que vos mails.

**Q3** - Écrivez le Dockerfile pour vos microservices (details, productpage et ratings). Le Dockerfile de reviews est déjà présent. Utilisez des images officielles. Aidez-vous des informations ci-dessous :
-   details

-   Utilisez ruby 2.7.1

-   Définissez deux variables d’environnement

-   SERVICE_VERSION (valeur par défaut v1)

-   ENABLE_EXTERNAL_BOOK_SERVICE (valeur par défaut false)

-   Faites en sorte que ces deux variables soient personnalisables lors du docker build

-   Le dossier de travail est /opt/microservices (là ou vous copierez les sources)

-   Pour exécuter votre service, la commande est “ruby details.rb 9080”

-   N’oubliez pas d’indiquer le port d’écoute

-   productpage

-   Utilisez python 3.7.7

-   Définisez la variable FLOOD_FACTOR (valeur par défaut 0, modifiable lors du build Docker)

-   Installez en premier les dépendances du code python, utilisez la commande “pip install –no-cache-dir -r <fichier-requirements.txt>”. 

-   Copiez le code restant dans /opt/microservices

-   La commande de démarrage est python productpage.py 9080

-   ratings

-   Utilisez node 12.18.1

-   Installez les dépendances via “npm install” et le fichier package.json

-   Copiez le code dans /opt/microservices

-   Ajoutez les dépendances systèmes grâce à la commande “apt-get update && apt-get install curl --no-install-recommends -y && rm -rf /var/lib/apt/lists/*”

-   Ajoutez la variable SERVICE_VERSION (valeur par défaut v1, éditable au docker build)

-   Commande de démarrage “node ratings.js 9080”

N’oubliez pas de bien choisir l’ordre de vos instructions Dockerfile, afin de bénéficier au maximum du cache de Docker.

  
  Pour trouver les bons tags on va sur docker en ligne pour python et on recherche dans l'onglet "Tags" la version qu'on veut
  
  -> Pour ***productpage***
  
  Pour faire la val par défaut dans un env il faut ajouter un arg à lui passer
  
  Il faut prendre la version de l'image la plus précise possible
  
  Copier les autres fichiers dans /opt/microservices = copier les fichiers de notre dossier productpages dans le conteneur dans le dossier indiqué
  
  RUN pour lancer des commandes avant CMD
  
  WORKDIR pour que toutes les commandes se lancent dans ce dossier
  
  Alpine a pas le compileur C donc ça fait plein d'erreurs donc on préfère prendre une image autre qu'alpine
  
  Organiser du moins changeant au plus changeant
  donc FROM en premier puis WORKDIR et ADD car peut de chance qu'ils changent puis arguments de build (ARG et ENV) puis RUN car peut changer souvent puis CMD en dernier comme toujours
  
  `docker build -t productpage .`
  --> pour construire l'image
  `docker run -it productpage`
  -> pour la lancer
  
  *Pour Git, afin de merger la branche créée :*
  - `git pull --rebase origin main`
     --> rebase pour faciliter le merge car d'autres branche ont été poussées
  - `git push origin +productpage`
  --> forcer le push pour réécrire l'historique
  
  

**Q4** -  Buildez vos conteneurs, et tentez de les démarrer un par un pour valider leur fonctionnement. Vous aurez peut-être besoin de démarrer les bases mongodb et mysql, présentes dans src/

 `docker build -t productpage .`
  --> pour construire l'image
  `docker run -it productpage`
  -> pour la lancer



<br/>

---

# TP2

**Q1** - Installer docker compose

**Q2** - 
Démarrer WP, après avoir récupéré le projet :
`docker compose up -d`
`docker compose down`

**Q3** -
On a ajouté build dedans pour tout build automatiquement 

*Note : docker compose surtout pour les dev, en prod on utilise autre chose*

- links permet de dire avec qui on est dans notre réseau, donc avec qui on peut communiquer

- on définit une variable d'environnement avec environment

- pour la définition des ports, on fait "9080:9080" si on veut accéder depuis la machine hote et juste "9080" pour donner un port de sortie du conteneur

- args != environment

- le nom du service est mappé en DNS directement

Erreur de reviews, on va mettre l'image: docker.io/istio/examples-bookinfo-reviews-v1:latest


<br/>

---
# TP3

**Q1** - Installation kubectl

https://kubernetes.io//home/jbachelard/Documents/isima/rapport_mifr/docs/tasks/tools/install-kubectl/


**Q2** - Installation kind

https://kind.sigs.k8s.io/docs/user/quick-start/#installing-from-release-binaries

Permet de simuler un cluster pour pouvoir utiliser kubectl

`kind create cluster`
--> Créer un cluster pour pouvoir faire les commandes avec kubeclt



**Q3** - Commencez par lister les Namespaces du cluster, quelle  commande kubectl faut-il exécuter ?

Listez ensuite les éléments suivants :

-   Les pods dans le namespace default
    
-   Les pods dans le namespace kube-system
    

Quel est selon vous le rôle du namespace kube-system ?

`kubectl get namespace`

`kubectl get pods --namespace=default`
--> No resources found

`kubectl get pods --namespace=kube-system`
*the namespace for objects created by the Kubernetes system* -> sert à gérer Kubernetes 


**Q4** - Écrivez un manifest (fichier .yaml) pour démarrer le pod suivant :

-   1 seul conteneur avec l’image gcr.io/kuar-demo/kuard-amd64:blue
    
-   Le conteneur écoute sur le port 8080 en http

Pour écrire les manifests, il y a mieux que Stackoverflow : [la documentation de référence de l’API Kubernetes](https://kubernetes.io/docs/reference/kubernetes-api/workload-resources/pod-v1/).


```
apiVersion: v1
kind: Pod
metadata:
  name: pod-1
spec:
  containers:
  - name: container-1
    image: gcr.io/kuar-demo/kuard-amd64:blue
    ports:
    - name: http
      containerPort: 8080

```


**Q5** - Appliquez votre manifest au cluster via kubectl apply -f. Vérifiez que votre Pod a bien démarré. Trouvez un moyen d’accéder au port 8080 de votre conteneur (kubectl peut surement vous aider).

`kubectl apply -f manifest.yaml`

`kubectl get pods --namespace=default`

`kubectl port-forward pod/pod-1 8080:8080`

On peut accéder au pod sur localhost:8080


**Q6** - Supprimez votre Pod et modifiez votre manifest afin d’en faire un Deployment (aidez vous de la doc de référence). Appliquez votre fichier, puis essayez de changer la valeur du champ replicas  et de réappliquer votre fichier. Observez les pods dans votre cluster, que constatez vous ?

```
apiVersion: apps/v1
kind: Deployment
metadata:
  name: deployment-1
  labels:
    app: kuar
spec:
  replicas: 3
  selector:
    matchLabels:
      app: kuar
  template:
    metadata:
      labels:
        app: kuar
    spec:
      containers:
      - name: container
        image: gcr.io/kuar-demo/kuard-amd64:blue
        ports:
        - containerPort: 8080
```

*spec > selector > matchLabels doit être identique à template > metadata > labels*

Replicas va créer plusieurs copies créées sur la base de template :
 -> les pods sont donc dupliqués, mais ont chacun un nom unique

`kubectl get deploy` -> 5/5 available

Pour exposer tout les pods d'un coup pour faire du load balancing : 
`kubectl expose deploy/deployment-1 --name=service-1 --type=NodePort`

-> Type node port pour lier le port à un autre port (entre 31000 et 32000 par défaut mais on peut le préciser)

Pour se connecter, ça va pas être sur localhost mais sur l'adresse IP externe du conteneur avec le port externe donné :
`kubectl get nodes -o wide` --> on prends kind-control-plane
--> Sans kind il n'y aurais pas de soucis pour aller direct depuis le localhost

## Fil rouge
**Q7** - À la racine de votre projet, créez un dossier qui contiendra vos manifests kubernetes. Ajoutez ensuite un fichier .yaml par microservice qui compose votre application.
 Facile

**Q8** - Déterminez les composants Kubernetes nécessaires à l'exécution de vos services. Écrivez chacun des manifests correspondant (en ajoutant bien les informations comme le port des conteneurs). N’oubliez pas les labels.

On choisi de faire directement des deployments plutôt que des pods pour prévoir l'évolution pendant les futurs TP.

Docker-compose seulement en environnement de dev, en prod on ne l'utilise plus du tout donc il faut pousser les images créées sur le registre d'images de kind et on y aura accès dans les manifests

Le registre est local au projet.

On a fait un pod registry avec l'image registry:latest
`kubectl port-forward pod/registry 5000:5000`
--> A faire à chaque redémarrage

`docker build -t localhost:5000/details:latest src/details`
--> Pareil pour toutes les images crées dans le docker compose
Sauf celle de reviews car elle est à prendre à distance donc on la met direct dans le manifest yaml

`docker push localhost:5000/details:latest`
--> pour pousser les images locales sur le registre


Dans nos manifests, on peut accéder aux images composées comme ça:
`image: localhost:5000/details:latest`

Finalement, on a opté pour cette solution :
Pour éviter de s'embeter avec un registry interne, on fait une méthode avec kind directement. Il faut juste load les images quand elles changent.
`docker build -t details:latest src/details`
`kind load docker-image details:latest`

Dans le manifest le nom de l'image est *details:latest*
On ajoute aussi une ligne *imagePullPolicy: IfNotPresent* pour dire qu'au démarrage du pod, pas besoin d'aller pull une image sur un repo distant



**Q9** - Appliquez vos manifests avec kubectl apply -f  sur l’ensemble de votre dossier. Vérifiez que vos Pods sont bien créés

`kubectl apply -f manifests`
--> Tout les pods sont indiqués comme créés
Vérification avec 
`kubectl get pods`

<br/>

---
# TP4

**Q1 et Q2** -
On créé des objets services dans chacun de nos manifests. On laisse leurs types par défaut sauf pour product page qui a un *Node port* car c'est le seul auxquel on peut avoir accès depuis l'extérieur.

Le selector des services reprends le selector défini dans le pod correspondant

Après apply, tout est bien créé

`kubectl get svc`
--> Afficher les services et les ports associés

Pour récupérer l'adresse du docker exposé à l'extérieur (productpage) :
`kubectl get nodes -o wide`

En se connectant à l'adresse donnée et au port 32009 via un navigateur ça fonctionne


**Q3** -
La BDD mongodb ne doit pas utiliser l'image qu'on construit mais une autre donc on change notre champ "image"

On rajoute un volume et un volume Mount (dans le container) avec le même chemin dans les deux (/data/db)


**Q4**
Pareil que la Q3 pour la config du volume et le changement d'image

Pour le secret, il faut convertir le username et le mot de passe en base 64 : 
`echo -n 'root' | base64`
Puis créer le fichier secret.yaml pour les définir

*OU*

Créer un secret avec kubectl 
`kubectl create secret generic mysql-root-pwd --from-literal=username=root --from-literal=password=root`
`kubectl get secret mysql-root-pwd -o yaml` -> copier le contenu dans secrets.yaml

Pour la communication des pods entre eux -> ne pas nommer les service "service-truc" mais seulement "truc" : les pods communiquent entre eux grâce aux résolutions dns vis à vis du nom de service.

**Q5** -
Augmentation du nombre de réplique à 5 :
`kubectl get deploy` -> indique 5/5

Pour passer ratings vers la v2 il faut ajouter les variables d'env pour le connecter à la db, on choisit *mysql* :
```yaml
env:  
- name: SERVICE_VERSION  
  value: v2  
- name: DB_TYPE  
  value: mysql  
- name: MYSQL_DB_HOST  
  value: mysql  
- name: MYSQL_DB_USER  
  valueFrom:  
    secretKeyRef:  
      name: mysql-root-pwd  
      key: username  
- name: MYSQL_DB_PASSWORD  
  valueFrom:  
    secretKeyRef:  
      name: mysql-root-pwd  
      key: password
```
   

**Q6** - 
On ajoute un deploy pour la v2 avec egalement 5 répliques.
On ajoute ensuite des labels sur le template des deploy : `version: 'x'`, x étant 1 ou 2 en fonction du deploy.  
On lie ensuite le service au bon deploy en ajoutant le label au selector du service `version: 'x'`, x étant la version souhaitée.  
Pour faire un déploiement blue/green, il suffit de modifier la version sélectionnée sur le service.  
On a testé avec un pod par déploiement : on refresh la page, et avec un `kubectl get logs ...` on regarde si la bonne version a bien été appelée.

**Q7** -
On est allé voir dans le code de chaque projet où étaient les endpoints associés au liveness probe et readness probe. Tous avaient un livenessprobe à *\health*, mais aucun n'avait de readness probe, on a donc choisit le chemin de base de chaque application (/productpage, /reviews, etc.).






