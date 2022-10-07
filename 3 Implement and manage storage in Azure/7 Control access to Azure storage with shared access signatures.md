# [Control access to Azure storage with shared access signatures](https://learn.microsoft.com/en-us/training/modules/control-access-to-azure-storage-with-sas/)

## Learning objectives

* Identify the features of a shared access signature for Azure Storage.
* Identify the features of stored access policies.
* Programmatically generate and use a shared access signature to access storage.

## Authorization options for Azure Storage (blobs)

### Public access - "anonymous public read access"

To enable this, the setting must be configured at both storage account as well as container level - setting only one to public will not make the contents public. This setting cannot be controlled at blob level.

### Azure AD with OAuth

### Shared key

Azure creates two 512-bit access keys for every storage account. Share these keys with a client to grant (root level) access to the account. Recommended to combine with Azure Key Vault to enable regular key rotation for improved security.

### SAS

* User delegation SAS: Blob storage only, secured with AD
* Service SAS: Blob/Queue/Table/File, secured with a storage account key
* Account SAS: Same as service SAS, but can also get and control Service-level information

## Delegate access with SAS - best practices

* Always use HTTPS
* Most secure is user delegation, use it whenever possible (only for blobs)
* Set expiration time to smallest reasonable value
* Apply least privilege
* For high-risk scenarios, create a middle-tier service to manage users and their access to storage

## Lab

```bash
# create storage account 
az storage account create \
    --name medical-records \
    --access-tier hot \
    --kind StorageV2 \
    --resource-group [sandbox resource group]

# create container
az storage container create \
    --name patient-images \
    --account-name medical-records \
    --public-access off

# upload something to the container
az storage blob upload-batch \
    --source sas \
    --destination patient-images \
    --account-name medical-records \
    --pattern *.jpg

# get connection string for the account
az storage account show-connection-string --name medical-records
```

Sample connection string returned from Azure:

```json
{
  "connectionString": "DefaultEndpointsProtocol=https;EndpointSuffix=core.windows.net;AccountName=medical-records;AccountKey=super-secret-key-here"
}
```

## Delegate access with stored access policies

Revoking individual SAS isn't possible, and regenerating the keys of the entire storage account effectively revokes *all* of them. Instead, it is better to associate your SASs with a stored access policy.

Stored Access Policies can be defined on all four storage resources - blobs, file shares, queues, and tables. The parameters you can define for a stored access policy are:

* Identifier: Name of the stored access policy
* Start time: A DateTimeOffset value for the date and time when the policy might start to be used. This value can be null.
* Expiry time: A DateTimeOffset value for the date and time when the policy expires. After this time, requests to the storage will fail with a 403 error-code message.
* Permissions: The list of permissions as a string that can be one or all of ```acdlrw```.

![add policy in portal](/static/5-add-a-policy.png)

```shell
az storage container policy create \
    --name <stored access policy identifier> \
    --container-name <container name> \
    --start <start time UTC datetime> \
    --expiry <expiry time UTC datetime> \
    --permissions <(a) add, (c) create, (d) delete, (l) list, (r) read, or (w) write> \
    --account-key <storage account key> \
    --account-name <storage account name> \
```
