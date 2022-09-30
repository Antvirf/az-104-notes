# [Create an Azure Storage account](https://learn.microsoft.com/en-us/training/modules/create-azure-storage-account/)

## Learning objectives

* Decide how many storage accounts you need for your project
* Determine the appropriate settings for each storage account
* Create a storage account using the Azure portal

## How many storage accounts do you need?

Storage account is a container that groups a set of Azure Storage **DATA** resources together. Storage accounts are like regular Azure resources, and are visible in resource groups. It is wroth repeating that only **DATA** services are under a storage account; many 'storage' services are not.

What a storage account can contain:

* Azure Blobs
* Azure Files
* Azure Queues
* Azure Tables

What it **cannot** contain:

* Cosmos DB
* Azure SQL
* other resources etc.

### What is defined at storage account level

* Subscription (where it is billed)
* Location/data center
* Performance (standard or premium)
* Replication strategy
* Access tiers
* Secure transfer enforcement setting

Hence, for requirements that need different levels/config for each of the above options, separate storage accounts are needed. All of these options also affect cost; so it is more efficient to have more accounts if some data can use cheaper option(s). More accounts of course mean more administrative overhead, so have to find a balance.

## Account settings

* **Deployment model**: Resource manager (current, use only this) vs. Classic (legacy only, doesn't support resource groups)
* **Account kind**: Determines types of storage allowed and pricing:
  * StorageV2 (GPv2) - all types and latest features
  * StorageV1 (GPv1) - legacy for all types, but not all features
  * Blob storage - legacy that only allows **block blobs** and **append blobs**

## Account creation options

Usually it is a one time thing and it is therefore fine to use the portal for it.

* Azure Portal
* Azure CLI
* Azure PowerShell
* Management client libraries
