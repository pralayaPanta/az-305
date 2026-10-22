---
tags:
  - studynote
  - az-305
title:
author: Pralaya Panta
updated: 2026-10-21
---

```table-of-contents
```


## Microsoft Entra Domain Services

- provides managed domain services (Windows Server Active Directory domain join, Group Policy, LDAP / LDAPS, and Kerberos / NTLM authentication) inside an Azure Virtual Network without deploying, managing, or patching Domain Controller VMs

- When to recommend
	- Lift-and-shift legacy on-premises applications migrated to Azure IaaS VMs (or container workloads) that cannot use modern cloud authentication protocols (SAML 2.0, OAuth 2.0, OpenID Connect) and strictly require LDAP bind, NTLM, or Kerberos authentication
	- cloud-only organizations that have no on-premises Active Directory infrastructure but need traditional AD domain join and Group Policy management for Azure VMs (such as Azure Virtual Desktop session hosts)

- Architecture & Deployment
	- deploys 2 managed Windows Server domain controllers into a dedicated subnet (`At least /24` recommended; never mix other workloads in the Domain Services subnet)
	- managed automatically by Microsoft (OS patching, backups, domain controller health monitoring, and replication)
	- SKUs:
		- Standard: for small-to-medium environments (backup frequency every 5 days; 1 replica set)
		- Enterprise: supports multiple Replica Sets (up to 5 total) and daily backups
		- Premium: highest LDAP query throughput, daily backups, and up to 5 Replica Sets

- Synchronization Model (One-Way Sync Only)
	- synchronizes user accounts, group memberships, and password hashes (NTLM and Kerberos hashes) one-way from Microsoft Entra ID into Microsoft Entra Domain Services
	- changes made directly inside the managed domain (such as new custom OUs, service accounts, or Group Policy Objects) do not sync back to Microsoft Entra ID or on-premises AD
	- for hybrid users synced from on-premises AD via Entra Connect / Cloud Sync, NTLM & Kerberos password hash synchronization must be enabled so users can authenticate against the managed domain

- High Availability & Geo-Resilience (Replica Sets)
	- Availability Zones: deploys the two domain controllers across separate Availability Zones automatically in supported regions
	- Replica Sets: allows extending a single Microsoft Entra Domain Services managed domain namespace across multiple Azure regions and peered Virtual Networks (each Replica Set adds 2 additional managed domain controllers in the target VNet/region for low-latency local authentication and regional disaster recovery)

- Administrative Permissions
	- does not grant `Domain Administrator` or `Enterprise Administrator` privileges (Microsoft manages the underlying domain controllers)
	- delegates administrative control via the `AAD DC Administrators` security group in Microsoft Entra ID (members can join machines, manage DNS, create custom OUs, and edit Group Policy Objects via RSAT tools)


[What is Microsoft Entra Domain Services?](https://learn.microsoft.com/en-us/entra/identity/domain-services/overview)
[Microsoft Entra Domain Services replica sets](https://learn.microsoft.com/en-us/entra/identity/domain-services/concepts-replica-sets)
