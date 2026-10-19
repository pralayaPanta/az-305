---
tags:
  - studynote
  - az-305
title:
author: Pralaya Panta
---

```table-of-contents
```


## Features
- manage secrets
	- tokens, passwords, certs, API key
- manage keys
	- create and control encryption key
- manage certificates
	- enroll, manage, and deploy public and private cert


- tiers
	- standard
		- encrypt data with software key
	- premium
		- uses hardware security module (HSM) protected keys


## Data Protection


- soft delete
	- prevent accidental deletion of key vault including keys, secrets & certs
	- can be configured to be between 7-90 days
	- when creating a new key vault soft delete is on by default
	- once enabled cannot be disabled

- purge protection
	- prevent deletion of key vault 
	- can recover items at any point during retention period
	- not enabled by default
	- can be enabled once soft delete has been enabled
	- for the object with purge protection - it cannot be deleted until retention period passes
	- can be set between 7-90 days
	- once purge protection is set it cannot be changed

- permitted purge
	- need following access
		- Owner
		- Key Vault Purge Operator




[Introduction to Azure Key Vault](https://learn.microsoft.com/en-us/azure/key-vault/general/overview)
[Basic concepts](https://learn.microsoft.com/en-us/azure/key-vault/general/basic-concepts)