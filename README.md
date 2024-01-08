# How to deploy Azure Container Instance (ACI) a .NET8 CRUD WebAPI Azure CosmosDB Microservice

## 1. Create an Azure Container Registry

We create Azure Container Registry service for uploading the .NET CRUD WebAPI docker image

![image](https://github.com/luiscoco/MicroServices_dotNET8_CRUD_WebAPI-CosmosDB-deployed_to_Azure_Container_Instance/assets/32194879/38684cb4-6405-4516-a2d5-8c2b5fd9dddb)

![image](https://github.com/luiscoco/MicroServices_dotNET8_CRUD_WebAPI-CosmosDB-deployed_to_Azure_Container_Instance/assets/32194879/a34a5018-1a6f-4dec-8182-5a763be38da0)

![image](https://github.com/luiscoco/MicroServices_dotNET8_CRUD_WebAPI-CosmosDB-deployed_to_Azure_Container_Instance/assets/32194879/e1604cbc-3bb6-43ea-b210-d6c6034c6f31)

![image](https://github.com/luiscoco/MicroServices_dotNET8_CRUD_WebAPI-CosmosDB-deployed_to_Azure_Container_Instance/assets/32194879/e0d0ab7e-ba80-4fd3-ba16-58913bc4b76e)

![image](https://github.com/luiscoco/MicroServices_dotNET8_CRUD_WebAPI-CosmosDB-deployed_to_Azure_Container_Instance/assets/32194879/bdccf3dc-114e-4758-bd1b-0c3996a91d9a)


## 2. Set the Admin User

We can enable the Admin User in the Azure Portal 

![image](https://github.com/luiscoco/MicroServices_dotNET8_CRUD_WebAPI-CosmosDB-deployed_to_Azure_Container_Instance/assets/32194879/3690cdf3-d6b3-4003-9716-6cbf0eee4a87)

Or we can enable the Admin User programmatically with Azure CLI

```
az acr update --name mymicroservicecontainer --resource-group myRG --admin-enabled true
```

Log in to Azure ACR

```
az acr login --name mymicroservicecontainer
```

## 3. Create the Docker image

```
docker build -t mymicroservicecontainer.azurecr.io/mymicroservicecontainer:v1 .
```

## 4. Push the Docker image

```
docker push mymicroservicecontainer.azurecr.io/mymicroservicecontainer:v1
```

## 5. Run the container Docker image

```
docker run -p 80:8080 mymicroservicecontainer.azurecr.io/mymicroservicecontainer:v1
```

## 6. Create the Azure Container Instance (ACI) 

```
az container create --resource-group myRG --name mycontainerinstance --image mymicroservicecontainer.azurecr.io/mymicroservicecontainer:v1 --cpu 1 --memory 1.5 --registry-login-server mymicroservicecontainer.azurecr.io --registry-username mymicroservicecontainer --registry-password pf/wdPCStBI7KLyQO8eGJvqzTm3QImHNFwXYjBzEVO+ACRAWicW2 --dns-name-label mymicroservicedns007 --ports 8080 --location westeurope
```

Also we can input the command in multiline

```

```

## 7. 



## 4. 






