# E5-CISSP
Zenner-Gibeaux Geoffrey Rendu



<!-- PROJECT LOGO --> <br /> <div align="center"> <h3 align="center">Projet Kubernetes : Applications Python</h3> <p align="center"> 


<!-- ABOUT THE PROJECT -->
## Struture du projet

Voici la structure du projet:


![alt text](Structure.png)


 Contenu du Manifest de production

``` yaml
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

 Contenu du Manifest de préproduction

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

### Pour déployer les applications:

1- Exécuter la commande suivante pour mettre en place vos application avec le manifest:

``` bash
k apply -f "nom du fichier.yaml" -n "nom du namespace"
```

2- AEt vous pouvez voir vos application déployer en faisant:

``` bash
k get all
```


 Docker Hub repositories

![alt text](dockerHub.png)

<p align="right"><a href="#readme-top">back to top</a></p>
