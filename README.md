# E5-CISSP
Zenner-Gibeaux Geoffrey Rendu




<!-- PROJECT LOGO --> <br /> <div align="center"> <h3 align="center">Projet Kubernetes : Applications Python</h3> <p align="center"> 


<!-- TABLE OF CONTENTS --> <details> <summary>Table des matières</summary> <ol> <li><a href="#structure-du-projet">Structure du projet</a></li> <li><a href="#configurations">Configurations</a></li> 
<li><a href="#etape-du-build">Étape du build</a></li> <li><a href="#logs">Logs</a></li> <li><a href="#quelques-interfaces">Quelques Interfaces</a></li> </ol> </details>



<!-- ABOUT THE PROJECT -->
## Struture du projet

Voici la structure du projet:
![alt text](Structure.png)


. Contenu du Manifest de production

``` bash
# Déploiement pour Flask
apiVersion: apps/v1
kind: Deployment
metadata:
  name: flask-app
spec:
  selector:
    matchLabels:
      app: flask-app
  template:
    metadata:
      labels:
        app: flask-app
    spec:
      containers:
      - name: flask-container
        image: toniocs/flask-app:prod
        imagePullPolicy: Never
        ports:
        - containerPort: 5000
---
# Service pour Flask (NodePort)
apiVersion: v1
kind: Service
metadata:
  name: flask-service
spec:
  type: NodePort
  selector:
    app: flask-app
  ports:
  - name: http
    protocol: TCP
    port: 80
    targetPort: 5000
    nodePort: 30006
  - name: https
    protocol: TCP
    port: 443
    targetPort: 8080
    nodePort: 30007
---
# Déploiement pour FastAPI
apiVersion: apps/v1
kind: Deployment
metadata:
  name: fastapi-app
spec:
  selector:
    matchLabels:
      app: fastapi-app
  template:
    metadata:
      labels:
        app: fastapi-app
    spec:
      containers:
      - name: fastapi-container
        image: toniocs/fast-api:prod
        imagePullPolicy: Never
        ports:
        - containerPort: 8000
---
# Service pour FastAPI (LoadBalancer)
apiVersion: v1
kind: Service
metadata:
  name: fastapi-service
spec:
  type: LoadBalancer
  selector:
    app: fastapi-app
  ports:
  - name: http
    protocol: TCP
    port: 80
    targetPort: 8000
  - name: https
    protocol: TCP
    port: 443
    targetPort: 8000
---
# Déploiement pour Third-App
apiVersion: apps/v1
kind: Deployment
metadata:
  name: third-app
spec:
  selector:
    matchLabels:
      app: third-app
  template:
    metadata:
      labels:
        app: third-app
    spec:
      containers:
      - name: third-app-container
        image: toniocs/third-app:prod
        imagePullPolicy: Never
        ports:
        - containerPort: 8080
---
# Service pour Third-App (NodePort)
apiVersion: v1
kind: Service
metadata:
  name: third-app-service
spec:
  type: NodePort
  selector:
    app: third-app
  ports:
  - name: http
    protocol: TCP
    port: 80
    targetPort: 8080
    nodePort: 30008
  - name: https
    protocol: TCP
    port: 443
    targetPort: 8080
    nodePort: 30009

```

. Contenu du Manifest de préproduction

``` bash
# Déploiement pour Flask
apiVersion: apps/v1
kind: Deployment
metadata:
  name: flask-app
spec:
  selector:
    matchLabels:
      app: flask-app
  template:
    metadata:
      labels:
        app: flask-app
    spec:
      containers:
      - name: flask-container
        image: toniocs/flask-app:preprod
        imagePullPolicy: Never
        ports:
        - containerPort: 5000
---
# Service pour Flask (NodePort)
apiVersion: v1
kind: Service
metadata:
  name: flask-service
spec:
  type: NodePort
  selector:
    app: flask-app
  ports:
  - name: http
    protocol: TCP
    port: 80
    targetPort: 5000
    nodePort: 30001
  - name: https
    protocol: TCP
    port: 443
    targetPort: 8080
    nodePort: 30004
---
# Déploiement pour FastAPI
apiVersion: apps/v1
kind: Deployment
metadata:
  name: fastapi-app
spec:
  selector:
    matchLabels:
      app: fastapi-app
  template:
    metadata:
      labels:
        app: fastapi-app
    spec:
      containers:
      - name: fastapi-container
        image: toniocs/fast-api:preprod
        imagePullPolicy: Never
        ports:
        - containerPort: 8000
---
# Service pour FastAPI (LoadBalancer)
apiVersion: v1
kind: Service
metadata:
  name: fastapi-service
spec:
  type: LoadBalancer
  selector:
    app: fastapi-app
  ports:
  - name: http
    protocol: TCP
    port: 80
    targetPort: 8000
  - name: https
    protocol: TCP
    port: 443
    targetPort: 8000
---
# Déploiement pour Third-App
apiVersion: apps/v1
kind: Deployment
metadata:
  name: third-app
spec:
  selector:
    matchLabels:
      app: third-app
  template:
    metadata:
      labels:
        app: third-app
    spec:
      containers:
      - name: third-app-container
        image: toniocs/third-app:preprod
        imagePullPolicy: Never
        ports:
        - containerPort: 8080
---
# Service pour Third-App (NodePort)
apiVersion: v1
kind: Service
metadata:
  name: third-app-service
spec:
  type: NodePort
  selector:
    app: third-app
  ports:
  - name: http
    protocol: TCP
    port: 80
    targetPort: 8080
    nodePort: 30002
  - name: https
    protocol: TCP
    port: 443
    targetPort: 8080
    nodePort: 30003


```


<!-- GETTING STARTED -->
## Etape du build

### Pour déployer les applications:

1- Exécuter la commande suivante pour mettre en place vos application:

``` bash
docker compose up -d
```

2- Après le build, tapez les commandes suivantes:

``` bash
docker images
docker ps
```
ou
``` bash
docker ps -a
```
. Screen présentant les conteneurs en cours d'exécution

![alt text](screen/dockerps.png)

Testez vos applications en local sur les ports suivants:
  <ul>
    <li><a href="#http://localhost:5084">http://localhost:5080</a></li>
    <li><a href="#http://localhost:5081">http://localhost:5081</a></li>
    <li><a href="#http://localhost:5082">http://localhost:5082</a></li>
    <li><a href="#http://localhost:5083">http://localhost:5083</a></li>


  </ul>

3- Pushez vos images sur le docker hub

. Connectez vous au docker hub:

``` bash
docker login
```

. Déplacer dans le répertoire de l'application pour exécuter les commandes suivantes:
``` bash
cd  nom_application
```

``` bash
docker build -t nom_image:tag_name .
```

``` bash
docker tag nom_image:tag: your_username/nom_image:tag_name_for_hub
```

``` bash
docker push your_username/nom_image:tag_name_for_hub
```

. Docker Hub repositories

![alt text](screen/repository.jpg)

<p align="right">(<a href="#readme-top">back to top</a>)</p>


<!-- INTERFACES -->
## Quelques interfaces

. Soft UI Dashboard3

![alt text](screen/Dashboard.png)

. Soft UI Dashboard

![alt text](screen/SoftDashborad.png)

. Flask Materiaal Dashboard

![alt text](screen/flask.png)


## Choix des frameworks


Flask Soft UI Design

Léger et facile à utiliser.
Fournit une interface utilisateur moderne et personnalisable.

Ecommerce Flask Stripe

Simplifie l'intégration des paiements avec Stripe.
Idéal pour les projets de commerce électronique.

## Diagramme Projet

![alt text](screen/schema.png)
