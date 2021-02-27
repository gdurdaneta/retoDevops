# RetoDevops

Se comentan las configuraciones realizadas par el reto Devops Chiper

#Parte 1
Resuelva el inconveniente para que el cluster pueda bajar imágenes públicas (clave Cloud NAT).

El inconveniente era que las subredes no estaba anunciada en las subredes para el Cloud Router
-El mismo se resolviendo tildando la opcion de * Anunciar todas las redes visibles para Cloud Router *




#Parte 2
Conéctese al cluster y cree tres namespaces con el siguiente formato (<Mi_nombre>-<ambiente>, donde ambiente es develop, staging y production.

Para este caso la configuracion efectuada fue la siguiente * kubectl create nombre-enviroment * siendo la solicitada por el reto:

kubectl create gerardo-production
kubectl create gerardo-staging
kubectl create gerardo-develop

para confirmar la creacion de los namespaces fuera la correcta se uso el comando
kubectl get namespaces




#Parte 3
Cree un secreto con un archivo (puede ser cualquier archivo) y llámelo my-service-account y póngale la llave key.json. Para montarlo en cualquier ambiente.
en este caso se crearon 2 archivos txt, uno bajo el nombre de username y el siguiente con el nombre de password
para crear el secret se uso el comando:

kubectl create secret generic my-accound-service --from-file=username.txt --from-file=password.txt




#Parte 4
Cree un configmap con variables de entorno para un despliegue en el entorno de producción. cree tres variables (My_env1, My_env2 y My_env3)

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
 este fue cargado bajo los comandos
 kubectl apply -n gerardo-production -f envars.yaml
 y se uso el comando para verificar las variables
 kubectrl exec enviromentsvars -- printenv
 
 
 
 
 
 #Parte 5 
 Cree un despliegue dentro del cluster en el namespace de producción (puede ser un nginx) 
 que monte el secreto del punto 3 y ponga las variables de entorno del punto 4.
 
 se crearon 2 archivos y fueron subidos con los siguientes comandos
 
kubectl apply -n gerardo-production -f service.yaml
kubectl apply -n gerardo-production -f deploy.yaml





#Parte 6
Cree un proyecto git público o privado con un código básico. Cree un jenkinsfile dentro de este repo que me permita construir, correr unit test si las tiene y crear una imagen de docker para ser publicada. (tranquilo esto no tiene que correr solo quiero saber si conoce el jenkinsfile y si este puede identificar que hacer a la hora de despliegue en cada uno de los ambientes).

No pude realizarlo
