# How to deploy Azure Container Instance (ACI) a .NET8 CRUD WebAPI Azure CosmosDB Microservice

## 1. Create an Azure Container Registry

We create a Container Registry service

![image](https://github.com/luiscoco/Azure_ACR_Upload_.NET_8_Web_API/assets/32194879/16399ba3-d529-4862-99ef-71713d08d594)

![image](https://github.com/luiscoco/Azure_ACR_Upload_.NET_8_Web_API/assets/32194879/3691d9c5-850c-4305-9de5-38fda83a8372)

![image](https://github.com/luiscoco/Azure_ACR_Upload_.NET_8_Web_API/assets/32194879/be13a2f2-7f66-43db-b30a-d2c6c9d14a4e)

![image](https://github.com/luiscoco/Azure_ACR_Upload_.NET_8_Web_API/assets/32194879/9fbbc617-f846-4c2d-b26d-5a813ed85e9a)

![image](https://github.com/luiscoco/Azure_ACR_Upload_.NET_8_Web_API/assets/32194879/30b1e0a2-ccea-4ef6-98ec-c93e3a69d0e3)


## 2. 

az acr update --name mymicroservicecontainer --resource-group myRG --admin-enabled true

az acr login --name mymicroservicecontainer

docker build -t mymicroservicecontainer.azurecr.io/mymicroservicecontainer:v1 .

docker push mymicroservicecontainer.azurecr.io/mymicroservicecontainer:v1

docker run -p 80:8080 mymicroservicecontainer.azurecr.io/mymicroservicecontainer:v1

az container create --resource-group myRG --name mycontainerinstance --image mymicroservicecontainer.azurecr.io/mymicroservicecontainer:v1 --cpu 1 --memory 1.5 --registry-login-server mymicroservicecontainer.azurecr.io --registry-username mymicroservicecontainer --registry-password pf/wdPCStBI7KLyQO8eGJvqzTm3QImHNFwXYjBzEVO+ACRAWicW2 --dns-name-label mymicroservicedns007 --ports 8080 --location westeurope

## 3. 



## 4. 






