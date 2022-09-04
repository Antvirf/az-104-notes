# [Configure Azure Active Directory](https://docs.microsoft.com/en-us/learn/modules/configure-azure-active-directory/)

## Learning objectives

* Define Azure AD concepts, including identities, accounts, and tenants.
* Describe Azure AD features to support different configurations.
* Understand differences between Azure AD and Active Directory Domain Services (AD DS).
* Choose between supported editions of Azure AD.
* Implement the Azure AD join feature.
* Use the Azure AD self-service password reset feature.

## Azure Active Directory

Azure Active Directory (AAD) is a multi-tenant cloud-based directory and identity management service. Supports user access to various resources and applications, including internal/on-prem apps and resources, as well as external resources like O365, the Azure Portal and other SaaS applications.

|Key Feature| Description
|-|-|
SSO|Azure AD provides Single Sign-On to various web apps both on the cloud and on-premise.
Ubiquitous device support|AAD works with every device and provides the same experience across devices.
Secure remote access|AAD enables secure remote access for on-prem web apps, and supports additional factors of authentication for MFA, conditional access policies, and managing access using groups.
Cloud extensibility|Takes the traditional AD service and expands it to cloud services with AAD.
Sensitive data protection|Identity protection for sensitive data and apps, including audit and activity logging and flagging potential vulnerabilities on resources and users.
Self-service support|AAD offers extensive self-service support for users like configuring MFA options, resetting passwords, all of which reduces admin toil and enhances security.

Each of these key features is recommended as part of an enterprise AAD configuration.

## AAD Concepts

|Concept|Definition|
|-|-|
Identity|An object that can be authenticated, for example a user or an application that has a password, a key, or  a certificate.
Account|Identity that has data associated to it. An identity can exist without an account, but an account cannot exist without an identity.
AAD Account|An identity that is created through AAD / Microsoft cloud. The "work or school account" that you often see as a log-in option is an AAD account.
Azure tenant (=directory)|Single, dedicated instance of AAD, representing a single organization. When a company signs up for a Microsoft cloud service, a new 'tenant' is automatically created. More can be treated as needed.
Azure subscription|Used to pay for Azure cloud services. Each subscription is linked to a single tenant, but multiple subscriptions may be created when needed.

## Azure AD vs. AD DS

Active Directory Domain Services (AD DS) is a Windows Server-based AD. It is one part of the Windows Active Directory suite of technologies, which includes:

* AD DS: AD Domain Services
* AD CS: AD Certificate Services
* AD LS: AD Lightweight Directory Services
* AD FS: AD Federation Services
* AD RMS: AD Rights Management Services

Azure AD is similar to AD DS, but not identical. Table below shows the key differences.

|Feature| Azure AD | AD DS|
|--|--|--|
Identity Solution |Full identity solution, designed for internet-based applications using HTTP/HTTPS.|Directory Service 'only' (primarily)
Queries: REST API|Yes|No
Queries: LDAP|No|Yes
Communication protocols|SAML, WS-Federation, OpenID/OAuth|Kerberos
Structure|Flat - no OUs or GPOs, though has its own ['administrative units'](https://docs.microsoft.com/en-us/azure/active-directory/roles/administrative-units) |Grouped hierarchy with OUs (org units) and GPOs (Group Policy Objects)
Service type|Managed service|Self-hosted/maintained

## Azure AD Editions / licensing

Each version is a superset of the lower tier. The bullet points/table are more or less mutually exclusive. Full pricing/tier info [here](https://azure.microsoft.com/en-gb/pricing/details/active-directory/).

* Free version
  * On-prem directory synchronization
* MS 365 (included with Microsoft 365)
  * Identity and Access management for MS 365 apps
  * Group access management
* Premium P1
  * Self-service group management
  * Cloud write-back/sync capability
  * Microsoft Identity Manager (on-prem identity and access mgmt suite)
* Premium P2
  * Azure AD identity protection: Risk-based conditional access
  * Privileged identity management allows restricting and monitoring admins and providing just-in-time access

|Feature|Free|MS 365|Premium P1|Premium P2
|--|--|--|--|--|
Directory Objects|500k|Unlimited|Unlimited|Unlimited
SSO|Unlimited|Unlimited|Unlimited|Unlimited
User and group management|✅|✅|✅|✅|
Core Identity and Access Management|✅|✅|✅|✅|
B2B Collaboration features|✅|✅|✅|✅|
MFA Features||✅|✅|✅|
Branding||✅|✅|✅|
Identity and Access Management for MS 365 apps||✅|✅|✅|
Self-service pw reset: cloud||✅|✅|✅|
Self-service pw reset: on-prem|||✅|✅|
Premium features (yes, this is actually what they say)|||✅|✅|
Dynamic groups|||✅|✅|
Hybrid Identities for both cloud and on-prem access|||✅|✅|
Advanced Group Access Management|||✅|✅|
Conditional Access|||✅|✅|
Identity Protection||||✅|
Identity Governance||||✅|

## Azure AD Join (in the context of a device)

When a device is 'joined' to AAD, it allows the organization to manage that device. This also enables SSO and seamless access to on-premises. Intended for organizations without an on-prem AD, it can also be used in other scenarios (e.g. branch offices). There are two ways to 'join' a device to AAD:

* Register: AAD provides the device with an identity, which can be used to enable/disable the device.
* Join: (Extension of registering the device) Also allows the organization to change the local state of the device. This allows a user to sign in to that device using a work/school account instead of a personal account.

Combining AAD Join with Mobile Device Management (MDM) provides additional device attributes to AAD, which enables features like conditional access rules. This allows you to enforce security and compliance standards across the organization.

## Self-Service Password Reset

* Allows admin to select whether SSPR is enabled for nobody / everybody / selected group
* Must allow at least one authentication method to reset the password. These include email, text, mobile app or security questions (the last of which can be configured so that every user *must* provide them during registration)
