# [Secure your Azure resources with RBAC](https://docs.microsoft.com/en-us/learn/modules/secure-azure-resources-with-rbac/)

## Learning objectives

* Verify access to resources for yourself and others
* Grant access to resources
* View activity logs of Azure RBAC changes

## Azure RBAC

RBAC offers SSO and access management over your Azure resources to every subscription attached to a particular AAD tenant.

RBAC offers works by **assignment** of the relevant **role definition** to a **security principal** (user, group or application) at a particular **scope** (MG, Sub, RG, resource). The specific **Actions** and **NotActions** of the role definition define what the user with that role can and cannot do to the resources in the relevant scope.

Roles are additive, and follow an **allow** model. **Actions** and **NotActions** are used only *within* that role to determine what actions are allowed. Therefore, if a user has two distinct roles, one with **NotAction** and one with an **Action** for the same thing, the net effect is that the user ***can*** execute that action.

![rbac example](../static/2-rbac-overview.png)

### Linkage between AAD Roles, Azure roles and Classic subscription admin roles

![rbac image](../static/2-azuread-and-azure-roles.png)

### RBAC in Azure Portal (Access control IAM)

![rbac in Azure portal](../static/2-resource-group-access-control.png)

### Check role assignments in Azure Portal

![check1](../static/4-my-permissions-menu.png)
![check2](../static/4-my-permissions-pane.png)
![check rg](../static/4-resource-group-role-assignment.png)

### RBAC Activity log in Azure Activity Log

![activitylog](../static/6-activity-log-portal.png)

