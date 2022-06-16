---
page_type: sample
languages:
  - java
products:
  - azure
  - service-connector
urlFragment: serviceconnector-webapp-keyvault-java
---

# Using Service Connector to connect Azure WebApp with Azure Keyvault

The repository offers the sample codes of connecting Azure Keyvault to Azure WebApp with `system managed identity`. Follow the [steps](#getting-started) below to create and verify the connection.

## Getting Started

### 1. Prerequisites

- An Azure account with an active subscription. [Create an account for free](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio).
- Install the <a href="/cli/azure/install-azure-cli" target="_blank">Azure CLI</a> 2.18.0 or higher to provision and configure Azure resources.
- Sign in to Azure with CLI command

```azurecli
az login
```
- JDK 11 or higher.
- Maven 3.8.0 or higher.

### 2. Clone the sample codes and build

Clone the sample repository:
```bash
git clone https://github.com/Azure-Samples/serviceconnector-webapp-keyvault-java.git
```
Go to the root folder of repository:
```bash
cd serviceconnector-webapp-keyvault-java
```
Build the package
```
mvn clean package -DskipTests
```

### 3. Create App Service 


Create the webapp:
```azurecli
# create resource group
az group create --name <myResourceGroupName> --location eastus
# create appservice plan
az appservice plan create -g <myResourceGroupName> -n <myPlanName> --is-linux --sku B1
# create webapp
az webapp create -g <myResourceGroupName> -n <myAppName> --runtime "JAVA|11-java11" --plan <myPlanName>
```

### 4. Create Azure Keyvault
```azurecli
az keyvault create -g <myResourceGroupName> -n <myVaultName>
az keyvault secret set --name connectionString --value testValue --vault-name <myVaultName>
```

### 5. Create the connection
```azurecli
az webapp connection create keyvault -g <myResourceGroupName> -n <myAppName> --tg <myResourceGroupName> --vault <myVaultName> --system-identity --client-type springboot
```

### 6. Deploy to Azure App Service
```azurecli
az webapp deploy -g <myResourceGroupName> -n <myAppName> --src-path ./target/keyvault-0.0.1-SNAPSHOT.jar --type jar
```

### 7. Validate the connection
Test that the app will get the secret from Keyvault by `curl https://<myAPpName>.azurewebsites.net/get`, you'll see `testValue`.

### 8. Cleanup the resource
```azurecli
az group delete -n <myResourceGroupName> --yes
```