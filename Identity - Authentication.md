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
- 

[Hybrid identity](https://learn.microsoft.com/en-us/entra/identity/hybrid/whatis-hybrid-identity)
[Microsoft Entra ID documentation](https://learn.microsoft.com/entra/fundamentals/)
[Microsoft Entra features and licenses](https://learn.microsoft.com/entra/fundamentals/licensing-requirements)
[Microsoft Entra ID best practices](https://learn.microsoft.com/azure/active-directory/fundamentals/identity-secure-score)
[Microsoft Entra ID architecture](https://learn.microsoft.com/azure/architecture/guide/identity/identity-guide)