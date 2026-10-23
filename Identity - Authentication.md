---
tags:
  - studynote
  - az-305
title: Identity - Authentication
author: Pralaya Panta
updated: 2026-10-23
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
	- primary domain format: `*.onmicrosoft.com`

### Administrative Units (Entra ID P1/P2)
- logical container of Entra users, groups, or devices used to restrict administrative scope (e.g., delegate `Helpdesk Administrator` only to users in the `APAC Administrative Unit`)
- Restricted Management Administrative Units: prevents even tenant-level Global Administrators or Privileged Authentication Administrators from modifying protected executive/sensitive accounts unless explicitly assigned to the AU

### Self-Service Password Reset (SSPR) & Password Writeback
- allows users to reset or unlock their passwords without helpdesk intervention
- Password Writeback: writes cloud password changes/resets back to on-premises Active Directory in real time (requires Entra ID P1/P2 and Entra Connect / Cloud Sync)


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
			```json
			{
			  "Name": "Custom Reader",
			  "Actions": [
			    "Microsoft.Resources/subscriptions/resourceGroups/read"
			  ],
			  "NotActions": [],
			  "DataActions": [],
			  "NotDataActions": [],
			  "AssignableScopes": [
			    "/subscriptions/{subscriptionId}"
			  ]
			}
			```

### Attribute-Based Access Control (ABAC)
- builds on Azure RBAC by adding role assignment conditions based on resource, principal, or environment attributes (e.g., allow `Storage Blob Data Reader` only if blob index tag `Project == Cascade`)
- reduces role assignment sprawl across thousands of containers/blobs


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


- delegated permission
	- app acts on behalf of signed-in user
	- effective permission is intersection of app permission and user permission
- application permission
	- app runs as a background service without a signed-in user
	- always requires admin consent

### Workload Identity Federation (Federated Credentials)
- allows external workloads (GitHub Actions, Azure DevOps, AKS Service Accounts, AWS/GCP workloads) to authenticate to Entra ID using OIDC tokens with zero secrets or certificates to rotate

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
	- Use Azure Key Vault with managed identities for Azure resources to enable Azure web app to access secret configuration values



## Authorizing Access to On-Premises Resources

- Microsoft Entra Application Proxy
	- publishes internal on-premises web applications externally without opening inbound firewall ports or deploying VPNs/DMZ reverse proxies
	- Architecture:
		- lightweight Application Proxy Connector on Windows Server
		- makes outbound TCP 443 connections only
		- deploy 2+ connectors in a Connector Group for high availability
	- Pre-Authentication: always recommend Microsoft Entra ID Pre-Authentication (blocks unauthenticated traffic at the Azure edge and enforces Conditional Access + MFA before traffic ever reaches on-premises)
	- Single Sign-On (SSO) to backend on-prem apps: supports Kerberos Constrained Delegation (KCD) (Integrated Windows Auth), Header-based, SAML, and Password Vaulting

- Microsoft Entra Private Access (Global Secure Access / ZTNA)
	- modern Zero Trust Network Access (ZTNA) replacement for traditional corporate VPNs
	- provides granular per-app, per-port TCP/UDP access to private on-premises and Azure VNet resources with Conditional Access enforcement on every connection

- Microsoft Entra Kerberos (Hybrid Azure Files Authentication)
	- enables hybrid synced users to access Azure Files (SMB) using Kerberos tickets issued directly by Microsoft Entra ID without requiring line-of-sight network connectivity to on-premises Domain Controllers


## Hybrid Authentication Methods


- Password Hash Synchronization (PHS)
	- syncs a hash-of-a-hash (SHA-256 + per-user salt, 1,000 HMAC-SHA256 iterations) of on-premises AD passwords to Microsoft Entra ID
	- authentication occurs entirely in the cloud (users can still sign in if on-premises AD or network goes offline)
	- required for Leaked Credentials detection in Microsoft Entra ID Protection
	- supports Seamless SSO

- Pass-Through Authentication (PTA)
	- validates user passwords directly against on-premises Active Directory
		- using lightweight PTA agents installed on-premises
		- outbound TCP 443 only
		- deploy 3+ agents for high availability
	- when to recommend:
		- strict regulatory/security requirement that password hashes must never leave on-premises
		- need immediate enforcement of on-premises AD user states, logon hours, and account lockouts without waiting for sync intervals
	- supports Seamless SSO; can enable PHS alongside PTA as an automated cloud disaster recovery fallback

- Federation (AD FS or 3rd-Party IdP)
	- Entra ID redirects user sign-in to an on-premises Federation Server (WS-Federation, SAML 2.0, OIDC)
	- highest infrastructure overhead
	- requires redundant AD FS servers + Web Application Proxy servers in DMZ
	- when to recommend:
		- legacy on-premises Smartcard / Certificate authentication
		- existing third-party MFA solutions tightly integrated with AD FS
		- complex multi-forest or non-AD identity stores requiring custom claim rules


## Hybrid Directory Synchronization Tools

- Microsoft Entra Cloud Sync
	- lightweight provisioning agent managed entirely from the Microsoft Entra admin center in the cloud
	- supports disconnected multi-forest AD environments (no network connectivity or forest trusts required between AD forests)
	- built-in high availability by deploying multiple active provisioning agents
- Microsoft Entra Connect Sync
	- full synchronization engine installed and managed on an on-premises Windows Server
		- when to recommend over Cloud Sync:
			- require Pass-Through Authentication (PTA)
			- require Device Synchronization (Microsoft Entra Hybrid Joined devices / Windows Hello for Business hybrid trust)
			- large directories (>100,000 objects per domain) or complex custom attribute filtering/transformation rules


## External Identities


- Microsoft Entra B2B Collaboration (Guest Users)
	- invites external partner users into your workforce tenant as `UserType == Guest`
	- guests sign in using their own home identity provider (another Entra tenant, Google, SAML/WS-Fed, Email OTP) while authorization is controlled in your resource tenant
	- Cross-Tenant Access Settings: allows trusting the partner tenant's MFA claims and Device Compliance / Hybrid Join claims so guests aren't prompted to register MFA twice
	- Billing model: Monthly Active Users (MAU) — first 50,000 MAUs per month are free (including Premium P1/P2 features when linked to an Azure subscription)


- Microsoft Entra B2B Direct Connect
	- establishes a mutual two-way trust with another Microsoft Entra organization via Cross-Tenant Access Settings
	- enables seamless collaboration (e.g., Teams Shared Channels) without creating Guest user objects in your directory

- Microsoft Entra External ID for Customers (CIAM — Replaces legacy Azure AD B2C)
	- dedicated customer-facing tenant for consumer and business customer applications
	- supports custom branded sign-up/sign-in user flows, self-service registration, and social identity providers (Google, Facebook, Apple, custom OIDC/SAML)
	- isolates consumer accounts completely from internal corporate workforce directory


[Hybrid identity](https://learn.microsoft.com/en-us/entra/identity/hybrid/whatis-hybrid-identity)
[Microsoft Entra ID documentation](https://learn.microsoft.com/entra/fundamentals/)
[Microsoft Entra features and licenses](https://learn.microsoft.com/entra/fundamentals/licensing-requirements)
[Microsoft Entra ID best practices](https://learn.microsoft.com/azure/active-directory/fundamentals/identity-secure-score)
[Microsoft Entra ID architecture](https://learn.microsoft.com/azure/architecture/guide/identity/identity-guide)