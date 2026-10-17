---
tags:
  - studynote
  - az-305
title:
author: Pralaya Panta
---

```table-of-contents
```




## Microsoft Entra ID

- implement Microsoft Entra ID as a cloud-only identity solution
- cloud-only identity solution provides both identity management and protection
	- RBAC
	- conditional access
	- access reviews
- bring on-premises identities into Microsoft Entra ID using Entra Connect or Connect cloud sync
- licensing tiers
	- Free
		- basic user and group management
		- directory sync
	- P1
		- CA, group based licensing, self service password reset
	- P2
		- identity protection and PIM
- tenant
	- tenant is instance of Microsoft Entra ID
	- can have multiple az subscriptions
	- custom domain names can be added
	- primary domain format: ***.onmicrosoft.com**


## Azure Role-Based Access Control (RBAC)

- can be applied at 4 levels
	- management group
	- subscription 
	- resource group
	- resource
- role assignments are inherited
- built in roles
	- owner, contributor, reader, user access administrator
- custom roles
	- own roles using JSON
	- includes
		- name, actions, notactions, dataactions, notdataactions, assignablescopes
	- `{
		  "Name": "Custom Reader",
		  "Actions": [ "Microsoft.Resources/subscriptions/resourceGroups/read" ],
		  "NotActions": [],
		  "AssignableScopes": ["/subscriptions/{subscriptionId}"]
		}`


## Service principals for applications

- app can be represented as
	- app objects
		- definition for an app
		- allows service to know how to issue tokens
		- exists in its home directory only - only one app object
	- service principals
		- instance of an app
		- reference to an app object
		- same app object can have multiple service principals (1 to many relationship)
		- when an app is given permission to access resources in a tenant (consent or registration) a service principal is created

- types of service principals
	- application service principals
		- local representation or app instance of global app object
		- created in each tenant where app is used
		- defines what app can do, who can access the app and what resources the app can access
	- managed identity
		- provide an identity for application to use
		- creates service principal when enabled
		- types - system and user assigned
		- cannot manage creds
	- legacy
		- legacy app that was created before app registration were introduced
		- can have credentials, service principal names, URL or other properties
		- does not have an associated app registration


## Managed Identities

- combines Microsoft Entra authentication and Azure role-based access control (RBAC)
- azure handles credential rotation and expiration in background
- can be enabled or disabled on an app at any time
- types
	- system assigned
		- an identity is created in Microsoft Entra tied to the lifecycle of that specific service instance
		- deleted automatically when service is deleted
		- only that resource can use that identity to request token from Entra ID
	- user assigned
		- can assign to one or more instances of azure service
		- managed separately from the resources that use it
		- are suited for workloads with resources that are recycled frequently but with consistent permissions
- example
	- Use Azure Key Vault with managed identities for Azure resources to enable  Azure web app to access secret configuration values


[Hybrid identity](https://learn.microsoft.com/en-us/entra/identity/hybrid/whatis-hybrid-identity)
[Microsoft Entra ID documentation](https://learn.microsoft.com/entra/fundamentals/)
[Microsoft Entra features and licenses](https://learn.microsoft.com/entra/fundamentals/licensing-requirements)
[Microsoft Entra ID best practices](https://learn.microsoft.com/azure/active-directory/fundamentals/identity-secure-score)
[Microsoft Entra ID architecture](https://learn.microsoft.com/azure/architecture/guide/identity/identity-guide)