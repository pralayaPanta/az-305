---
tags:
  - studynote
  - az-305
title: Identity - Azure Key Vault
author: Pralaya Panta
updated: 2026-10-23
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
	- Azure Managed HSM
		- fully managed, single-tenant, highly available FIPS 140-3 Level 3 HSM cluster
		- recommend when strict regulatory compliance requires dedicated single-tenant HSM hardware and full customer control over the HSM security domain


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


## Azure RBAC for Key Vault

- Access Control Models:
	- Azure RBAC for Key Vault:
		- provides unified ARM management and granular per-secret, per-key, or per-certificate data-plane scopes
	- Vault Access Policies (Legacy):
		- vault-wide permissions model (max 1,024 policies per vault; cannot scope permissions down to an individual secret, key, or certificate)
- Built-in Key Vault Data-Plane Roles (Least Privilege Breakdown):
	- Vault-Wide Roles:
		- `Key Vault Administrator`: full data-plane access to manage all keys, secrets, and certificates inside the vault (does not allow modifying the Key Vault ARM resource itself or assigning RBAC roles)
		- `Key Vault Reader`: read-only access to view metadata/properties of keys, secrets, and certificates only (cannot read secret values or private key material)
	- Secrets Roles (Passwords, Connection Strings, API Keys):
		- `Key Vault Secrets Officer`: perform any action on secrets (create, read secret values, update, delete, backup, restore, recover) except managing permissions
		- `Key Vault Secrets User`: read-only access to secret contents/values only (ideal least-privilege role for Managed Identities of App Services, Functions, or VMs reading connection strings/API keys)
	- Keys / Cryptography Roles:
		- `Key Vault Crypto Officer`: perform any action on cryptographic keys (create, delete, rotate, configure rotation policy, import, backup/restore) except managing permissions
		- `Key Vault Crypto User`: perform cryptographic operations using keys (`encrypt`, `decrypt`, `wrapKey`, `unwrapKey`, `sign`, `verify`) without the ability to create or delete keys
		- `Key Vault Crypto Service Encryption User`: least-privilege role specifically for Azure PaaS services (Storage Accounts, Azure SQL TDE, Managed Disks) using Customer-Managed Keys (CMK) — only allows `get`, `wrapKey`, and `unwrapKey`
		- `Key Vault Crypto Service Release User`: allows releasing exportable keys into attested Confidential Computing / Secure Enclaves
	- Certificates Roles (TLS/SSL Certificates):
		- `Key Vault Certificates Officer`: perform any action on certificates (create, import, manage CA issuers, update auto-renewal policy, delete) except managing permissions
		- `Key Vault Certificate User`: read-only access to full certificate contents (including private key/secret portion needed by App Service, Application Gateway, or Front Door for TLS binding)
- Key Vault Control-Plane Role (ARM Management Only):
	- `Key Vault Contributor`: manages the Key Vault resource in Azure Resource Manager (firewall rules, Private Endpoints, SKU tier, tags), but has zero access to read or modify secrets, keys, or certificates inside the vault when using the Azure RBAC permission model


## Key Rotation, Certificate Auto-Renewal & Key Vault References

- Automatic Key Rotation:
	- automatically generates a new cryptographic key version on a defined schedule and notifies downstream services via Azure Event Grid
- Certificate Auto-Renewal:
	- integrates directly with partnered Certificate Authorities (DigiCert, GlobalSign) to automatically renew TLS/SSL certificates before expiry
- Key Vault References (App Service & Azure Functions):
	- allows apps to read secrets via application settings (`@Microsoft.KeyVault(SecretUri=...)`) using a Managed Identity with zero code changes


[Introduction to Azure Key Vault](https://learn.microsoft.com/en-us/azure/key-vault/general/overview)
[Basic concepts](https://learn.microsoft.com/en-us/azure/key-vault/general/basic-concepts)
[Provide access to Key Vault keys, certificates, and secrets with an Azure role-based access control](https://learn.microsoft.com/en-us/azure/key-vault/general/rbac-guide)