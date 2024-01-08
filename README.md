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

## 3. Create a Dockerfile

With Visual Studio 2022 Community Edition we can automatically create the Dockerfile. 

After creating automatically the Dockerfile we **expose the port 80**

This is the Dockerfile source code

```
#See https://aka.ms/customizecontainer to learn how to customize your debug container and how Visual Studio uses this Dockerfile to build your images for faster debugging.

FROM mcr.microsoft.com/dotnet/aspnet:8.0 AS base
USER app
WORKDIR /app
EXPOSE 80

FROM mcr.microsoft.com/dotnet/sdk:8.0 AS build
ARG BUILD_CONFIGURATION=Release
WORKDIR /src
COPY ["AzureCosmosCRUDWebAPI.csproj", "."]
RUN dotnet restore "./././AzureCosmosCRUDWebAPI.csproj"
COPY . .
WORKDIR "/src/."
RUN dotnet build "./AzureCosmosCRUDWebAPI.csproj" -c $BUILD_CONFIGURATION -o /app/build

FROM build AS publish
ARG BUILD_CONFIGURATION=Release
RUN dotnet publish "./AzureCosmosCRUDWebAPI.csproj" -c $BUILD_CONFIGURATION -o /app/publish /p:UseAppHost=false

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "AzureCosmosCRUDWebAPI.dll"]
```

## 4. Create the Docker image

```
docker build -t mymicroservicecontainer.azurecr.io/mymicroservicecontainer:v1 .
```

![image](https://github.com/luiscoco/MicroServices_dotNET8_CRUD_WebAPI-CosmosDB-deployed_to_Azure_Container_Instance/assets/32194879/d3cf3bae-4791-46ca-8ade-680c1d71cd7d)

## 5. Push the Docker image

Log in to Azure ACR

```
az acr login --name mymicroservicecontainer
```

And we push the docker image to Azure ACR

```
docker push mymicroservicecontainer.azurecr.io/mymicroservicecontainer:v1
```

## 6. Verify the Docker image in Azure ACR

We navigate to the Azure ACR

![image](https://github.com/luiscoco/MicroServices_dotNET8_CRUD_WebAPI-CosmosDB-deployed_to_Azure_Container_Instance/assets/32194879/d01246fc-cb1d-4a6d-9353-dbb7145b3bbd)

![image](https://github.com/luiscoco/MicroServices_dotNET8_CRUD_WebAPI-CosmosDB-deployed_to_Azure_Container_Instance/assets/32194879/6aa03f7a-7478-4db8-8550-c4e9afbe9772)

![image](https://github.com/luiscoco/MicroServices_dotNET8_CRUD_WebAPI-CosmosDB-deployed_to_Azure_Container_Instance/assets/32194879/85f62764-9d7e-4653-9693-558fe1b15cfc)

![image](https://github.com/luiscoco/MicroServices_dotNET8_CRUD_WebAPI-CosmosDB-deployed_to_Azure_Container_Instance/assets/32194879/baa97e05-c998-4433-8dd8-c0b9d271815b)

We first pull the Azure Docker image to our local laptop with this command

```
docker pull mymicroservicecontainer.azurecr.io/mymicroservicecontainer:v1
```

And then we run the docker image

```
docker run -p 80:8080 mymicroservicecontainer.azurecr.io/mymicroservicecontainer:v1
```

We can access the application endpoint

http://localhost/api/family

![image](https://github.com/luiscoco/MicroServices_dotNET8_CRUD_WebAPI-CosmosDB-deployed_to_Azure_Container_Instance/assets/32194879/0e4a5204-7780-4e0d-b755-b225b86ab540)

## 7. Create the Azure Container Instance (ACI) 

We copy the ACR username and password:

![image](https://github.com/luiscoco/MicroServices_dotNET8_CRUD_WebAPI-CosmosDB-deployed_to_Azure_Container_Instance/assets/32194879/2099d8be-1613-44fb-90d1-08ed846fe0b6)

**ACR username**: mymicroservicecontainer
 
**ACR password**: RtxdWayhsBSsrvs9ZEpG/cV+M9A1F7Xu5td9+S2lWp+ACRDRt6Dk

```
az container create --resource-group myRG --name mycontainerinstance --image mymicroservicecontainer.azurecr.io/mymicroservicecontainer:v1 --cpu 1 --memory 1.5 --registry-login-server mymicroservicecontainer.azurecr.io --registry-username mymicroservicecontainer --registry-password RtxdWayhsBSsrvs9ZEpG/cV+M9A1F7Xu5td9+S2lWp+ACRDRt6Dk --dns-name-label mymicroservicedns007 --ports 8080 --location westeurope
```

Also we can input the command in multiline

```
az container create --resource-group myRG ^
--name mycontainerinstance ^
--image mymicroservicecontainer.azurecr.io/mymicroservicecontainer:v1 ^
--cpu 1 ^
--memory 1.5 ^
--registry-login-server mymicroservicecontainer.azurecr.io ^
--registry-username mymicroservicecontainer ^
--registry-password RtxdWayhsBSsrvs9ZEpG/cV+M9A1F7Xu5td9+S2lWp+ACRDRt6Dk ^
--dns-name-label mymicroservicedns007 ^
--ports 8080 ^
--location westeurope
```

## 8. Verify the application running in the Azure Container Instance (ACI)

We navigate to the Azure ACI service

![image](https://github.com/luiscoco/MicroServices_dotNET8_CRUD_WebAPI-CosmosDB-deployed_to_Azure_Container_Instance/assets/32194879/97d61999-ce3f-4eb4-af4b-e9f66a9fa2a1)

We press in the Azure ACI link

We copy the FQDN

![image](https://github.com/luiscoco/MicroServices_dotNET8_CRUD_WebAPI-CosmosDB-deployed_to_Azure_Container_Instance/assets/32194879/11223a06-d895-4c8b-aa59-2f0c80c21266)

We input the Azure ACI endpoint in the internet web browser



## 9. 






