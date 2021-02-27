# retoDevops

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
