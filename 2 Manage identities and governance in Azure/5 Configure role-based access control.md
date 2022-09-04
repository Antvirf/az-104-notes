# [Configure role-based access control](https://docs.microsoft.com/en-us/learn/modules/configure-role-based-access-control/)

## Learning objectives

* Identify the features and usage cases for role-based access control.
* List and create role definitions.
* Create role assignments.
* Identify the differences between Azure role-based access control and Azure Active Directory roles.
* Manage access to subscriptions using role-based access control.
* Review the built-in Azure role-based access control roles.

## Role-Base Access Control (RBAC) concepts

Default access it **none** - assigned roles **grant** access - they cannot be used to explicitly **deny**. Denying access is done via removing the assignment of the relevant role definition from the relevant security principal at the relevant scope.

However, the role definition itself may contain **NotActions** that deny certain things, as this is required to create permissions such as "edit every VM except X and Y", allowing for more fine-grained roles to be created.

|Concept|Definition|
|--|--|
Security principal|Object that represents something requesting access. For example: user, group, service principal, or a resource with a managed identity.
Role definition|Collection of permissions that lists the operations that can be performed. For example: Reader, Contributor, Owner, User ACcess Administrator.
Scope|Boundary for the level of access that is requested. For example: MG, Subscription, RG, Resource
Assignment|Attachment of a **role definition** to a chosen **security principal** at a particular **scope**.
Role def: Action|Specific actions to allow in a role def
Role def: NotAction|Specific actions to disallow in a role def
Role def: DataAction|Specific DataAction and NotDataACtions to allow/disallow in a role def

Example use cases:

* Allow an app or a user to access all resources within a RG
* Allow one user to manage VMs in a subscription, and another to manage vnets
* Allow a DBA group to manage all SQL DBs in a subscription

![role assignment example](../static/role-assignment-040eb1ab.png)

## Most common Azure roles

These four are the fundamental built-in roles. The first three apply to all resource types. An additional layer is provided by different pre-available roles defined for each service type: e.g. a **Virtual Machine *contributor***. Custom roles can also be created.

* **Owner**: Full access to all resources in scope, including ability to grant access to others. Service Administrator and Co-administrator are assigned this role at the subscription scope.
* **Contributor**: Full access to all resources in scope, but can **not** grant access to others.
* **Reader**: View access to existing resources in scope.
* **User access administrator**: Ability to manage user access to resources, but no access to resources themselves.

## Azure Roles vs. Azure AD Roles

|Azure RBAC Role|Azure AD Role|
|--|--|
Manage access to **Azure cloud resources** (e.g. a VM)|Manage access to **Azure AD resources** (e.g. a security group)
Scope can be specified at MG/Sub/RG/Resource level|Scope is always at the tenant level
Access: AzPortal/Cli/PwSh/ARM template, REST API|Access: AzPortal/O365Portal/Graph Azure AD PwSh

![rbac sample](../static/role-based-authentication-b3dda7ae.png)
