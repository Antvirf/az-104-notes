# [Configure resources with Azure Resource Manager templates](https://docs.microsoft.com/en-us/learn/modules/configure-resources-arm-templates/)

## Learning objectives

* List the advantages of Azure templates.
* Identify the Azure template schema components.
* Specify Azure template parameters.
* Locate and use Azure Quickstart Templates.

## ARM Templates

Declarative JSON-based (*or* 'Bicep'-based) templates specific to Azure that allow you to declare your infrastructure as code. They retain a connection between a deployment and the template - if you redeploy a slightly changed template, only edits are applied (i.e. it doesn't just re-deploy everything).

Many of the benefits below are applicable to Infrastructure-as-Code (IaC) generally, not just ARM templates.

* Improve consistency
* Express and execute complex deployments easily, including dependencies and order
* Templates are code, and can be tracked as code
* Promote reuse
* Linkable and modular

### JSON-based ARM templates

Example ARM template and mandatory/optional fields

```json
{
    "$schema": "http://schema.management.​azure.com/schemas/2019-04-01/deploymentTemplate.json#",  # REQUIRED
    "contentVersion": "",​       # REQUIRED
    "parameters": {},​           # optional: input parameters
    "variables": {},​            # optional
    "functions": [],            # optional
    "resources": [],            ​# REQUIRED
    "outputs": {}​               # optional: output parameters
}
```

### Bicep-based ARM templates

[Azure Bicep](https://docs.microsoft.com/en-us/azure/azure-resource-manager/bicep/overview?tabs=bicep) is a domain-specific language for declarative IaC on Azure. It is simpler in syntax compared to JSON, and is effectively an abstraction created on top of the JSON template - they have exactly the same capabilities.

Some advantages over JSON include:

* Simpler syntax
* Module files and automatic dependency management
* Type validation and intellisense in VSCode

Example ARM template in Bicep

```Bicep
param location string = resourceGroup().location
param storageAccountName string = 'toylaunch${uniqueString(resourceGroup().id)}'

resource storageAccount 'Microsoft.Storage/storageAccounts@2021-06-01' = {
  name: storageAccountName
  location: location
  sku: {
    name: 'Standard_LRS'
  }
  kind: 'StorageV2'
  properties: {
    accessTier: 'Hot'
  }
}
```

## Quickstart templates

ARM templates provided by the community. Available [here](https://azure.microsoft.com/resources/templates/).