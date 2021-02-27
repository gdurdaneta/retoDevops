# RetoDevops

Se comentan las configuraciones realizadas par el reto Devops Chiper, desglosadas en 6 estapas


# Etapa 1
####Desafio
Resuelva el inconveniente para que el cluster pueda bajar imágenes públicas (clave Cloud NAT).

####Solucion
El inconveniente consistia que las subredes no estaban anunciada en las subredes para el Cloud Router
El mismo se resolviendo tildando la opcion de *** Anunciar todas las redes visibles para Cloud Router ***

![](https://raw.githubusercontent.com/gdurdaneta/retoDevops/main/Captura%20de%20pantalla%20de%202021-02-27%2015-39-59.png)




# Etapa 2
####Desafio
Conéctese al cluster y cree tres namespaces con el siguiente formato (<Mi_nombre>-<ambiente>, donde ambiente es develop, staging y production.

####Solucion

Para este caso la configuracion efectuada fue la siguiente * kubectl create nombre-enviroment * siendo la solicitada por el reto:

kubectl create gerardo-production
kubectl create gerardo-staging
kubectl create gerardo-develop

Para confirmar la creacion de los namespaces fuera la correcta se uso el comando
kubectl get namespaces


# Etapa 3
####Desafio

Cree un secreto con un archivo (puede ser cualquier archivo) y llámelo my-service-account y póngale la llave key.json. Para montarlo en cualquier ambiente.

####Solucion

En este caso se crearon 2 archivos txt, uno bajo el nombre de username y el siguiente con el nombre de password, para crear el secret se uso el comando:

*kubectl create secret generic my-accound-service --from-file=username.txt --from-file=password.txt*


# Etapa 4
####Desafio
Cree un configmap con variables de entorno para un despliegue en el entorno de producción. cree tres variables (My_env1, My_env2 y My_env3)

####Solucion

Se creo un archivo llamado envars.yaml donde contenia el siguiente codigo

```
apiVersion: v1
kind: Pod
metadata:
  name: envarsreto
  labels:
    purpose: enviromentvars
spec:
  containers:
  - name: envar-demo-container
    image: gcr.io/google-samples/node-hello:1.0
    env:
    - name: myenv1
      value: nginx
    - name: myenv2
      value: "80"
    - name: myenv3
      value: value
 ```
 
 Este fue cargado bajo los comandos **kubectl apply -n gerardo-production -f envars.yaml** y fue usado el comando para verificar las variables ** kubectrl exec enviromentsvars -- printenv**
 

 # Etapa 5 
 #### Desafio

 Cree un despliegue dentro del cluster en el namespace de producción (puede ser un nginx) 
 que monte el secreto del punto 3 y ponga las variables de entorno del punto 4.
 
  #### Solucion
 
En este caso se crearon los archivos service.yaml y deploy.yaml y fueron aplicados a los entornos de producion de gerardo-production, con los siguientes comandos:

service.yaml
```
apiVersion: v1
kind: Service
metadata:
  name: serivenginx
  labels:
    app: myenv1
spec:
  type: NodePort
  selector:
    app: myenv1
  ports:
    - protocol: TCP
      targetPort: 80
      port: 80
      nodePort: 30080
```      
      
  kubectl apply -n gerardo-production -f service.yaml

  y deploy.yaml

 # Etapa 5 
 #### Desafio


```
apiVersion: apps/v1
kind: Deployment
metadata:
  labels:
    app: myenv1
  name: myenv1
spec:
  replicas: 1
  selector:
    matchLabels:
      app: myenv1
  template:
    metadata:
      labels:
        app: myenv1
    spec:
      containers:
      - image: nginx:latest
        name: nginx
```

kubectl apply -n gerardo-production -f deploy.yaml


# Etapa 6
 #### Desafio
Cree un proyecto git público o privado con un código básico. Cree un jenkinsfile dentro de este repo que me permita construir, correr unit test si las tiene y crear una imagen de docker para ser publicada. (tranquilo esto no tiene que correr solo quiero saber si conoce el jenkinsfile y si este puede identificar que hacer a la hora de despliegue en cada uno de los ambientes).

 #### Solucion

se diseño este pipeline donde se describen las etapas mas basicas de compila, probar y entregar, para aplicaciones de Node.js y react mas complejas 

```
pipeline {
    agent {
        docker { # 
            image 'node:6-alpine' 
            args '-p 3000:3000' 
        }
    }
    environment {
        CI = 'true'
    }
    }
    stages {
        stage('Build') { 
            steps {
                sh 'npm install' 
            }
        }
        stage('Test') {
            steps {
                sh './jenkins/scripts/test.sh'
            }
        }
        stage('Deliver') {
            steps {
                sh './jenkins/scripts/deliver.sh'
                input message: 'Finished using the web site? (Click "Proceed" to continue)'
                sh './jenkins/scripts/kill.sh'
            }
        }
    }
}
```
