# [Control Azure services with the CLI](https://docs.microsoft.com/en-us/learn/modules/control-azure-services-with-cli/)

## Learning objectives

* Install the Azure CLI on Linux, macOS, and/or Windows
* Connect to an Azure subscription using the Azure CLI
* Create Azure resources using the Azure CLI

## Installation

* Ubuntu: ```apt-get```
* Mac: ```Homebrew```

## Azure CLI Usage

* Searching for commands to use: ```az find```
* Authentication: ```az login```

## Example: Basic commands

```bash
# create resource group
az group create --name <name> --location <location>

# list resource groups, with optional formatter argument
az group list
az group list --output table

```

## Example: Creating a website

```bash
# define variables for the example
export RESOURCE_GROUP=["sandbox resource group name"]
export AZURE_REGION="centralus"
export AZURE_APP_PLAN=popupappplan-$RANDOM
export AZURE_WEB_APP=popupwebapp-$RANDOM

# create app service plan
az appservice plan create --name $AZURE_APP_PLAN --resource-group $RESOURCE_GROUP --location $AZURE_REGION --sku FREE

# check it was created
az appservice plan list --output table

# then, create the webapp
az webapp create --name $AZURE_WEB_APP --resource-group $RESOURCE_GROUP --plan $AZURE_APP_PLAN

# check it was created
az webapp list --output table

# check it is up
curl $AZURE_WEB_APP.azurewebsites.net

# deploy to the webapp from a GitHub repo
az webapp deployment source config \
    --name $AZURE_WEB_APP \
    --resource-group $RESOURCE_GROUP \
    --repo-url "https://github.com/Azure-Samples/php-docs-hello-world" \
    --branch master \
    --manual-integration

# check deployment works
curl $AZURE_WEB_APP.azurewebsites.net

```
