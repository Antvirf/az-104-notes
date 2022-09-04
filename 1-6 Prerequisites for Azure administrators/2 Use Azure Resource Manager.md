# [Use Azure Resource Manager](https://docs.microsoft.com/en-us/learn/modules/use-azure-resource-manager/)

## Learning objectives

* Identify the features and usage cases for Azure Resource Manager.
* Describe each Azure Resource Manager component and its usage.
* Organize your Azure resources with resource groups.
* Apply Azure Resource Manager locks.
* Move Azure resources between groups, subscriptions, and regions.
* Remove resources and resource groups.
* Apply and track resource limits

### Moving resources

Resources can be moved across **subscriptions, resource groups and regions** but not without limitations. Full list of move operation support is available [here](https://docs.microsoft.com/en-us/azure/azure-resource-manager/management/move-support-resources).

Some notable resources and their moveability:

| Resource | Move x RG? | Move x Subscription? | Move x Region?|
|--|--|--|--|
|Kubernetes/AKS|⛔|⛔|⛔
|ACS|✅|✅|⛔|
|VM|✅|✅|✅|
|VNet|✅|✅|⛔|

Note that any child resource moves with the parent, and also inherits the moveability of the parent ***most of the time***. E.g. ```Microsoft.ServiceBus/namespaces/queues``` can't be moved independently of ```Microsoft.ServiceBus/namespaces```.

### Resource limits

Azure has certain pre-set limits (sometimes called quotas) for resources. Different quotas are defined at different levels, e.g. some are defined at MG level (e.g. MGs per tenant), some at Subscription level (RGs per subscription), some at RG level (Resource types per RG, only applicable to some types.)

* A limit can have a default value as well as a maximum value that is higher. To request for an increase, file a support ticket.
* Full details [here](https://docs.microsoft.com/en-us/azure/azure-resource-manager/management/azure-subscription-service-limits)
* You can view your limits in the portal: ![limit image](../static/check-resource-limits-4f522428.png)
