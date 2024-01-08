# How to deploy Azure Container Instance (ACI) a .NET8 CRUD WebAPI Azure CosmosDB Microservice

## 1. 


## 2. 

az acr update --name mymicroservicecontainer --resource-group myRG --admin-enabled true

az acr login --name mymicroservicecontainer

docker build -t mymicroservicecontainer.azurecr.io/mymicroservicecontainer:v1 .

docker push mymicroservicecontainer.azurecr.io/mymicroservicecontainer:v1

docker run -p 80:8080 mymicroservicecontainer.azurecr.io/mymicroservicecontainer:v1

az container create --resource-group myRG --name mycontainerinstance --image mymicroservicecontainer.azurecr.io/mymicroservicecontainer:v1 --cpu 1 --memory 1.5 --registry-login-server mymicroservicecontainer.azurecr.io --registry-username mymicroservicecontainer --registry-password pf/wdPCStBI7KLyQO8eGJvqzTm3QImHNFwXYjBzEVO+ACRAWicW2 --dns-name-label mymicroservicedns007 --ports 8080 --location westeurope

## 3. 



## 4. 






