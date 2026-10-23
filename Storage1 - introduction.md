---
tags:
  - az-305
  - studynote
title: Storage - introduction
author: Pralaya Panta
updated: 2026-10-23
---

```table-of-contents
```



## Data Types
- Structured
	- stored in a relational format that has a shared schema.
	- contained in a database table with rows, columns, and keys.
- Semi-structured
	- data fields don't fit neatly into tables, rows, and columns.
	- html, json, xml files
- Unstructured
	- mix of information without a clear relationship.
	- format of unstructured data is referred to as nonrelational.
	- text files like PDF, TXT, and RTF
	- media files like photos, videos, and audio


## Azure storage accounts
- unique namespace for storage data accessed over HTTPS
	- every storage account must have a unique account name within Azure
- data written to an Azure storage account is encrypted by the service.
- type
	- standard general purpose v2
	- premium block blobs
	- premium file shares
	- premium page blobs
- redundancy option
	- Locally redundant storage (LRS)
	- Geo-redundant storage (GRS)
	- Read-access geo-redundant storage (RA-GRS)
	- Zone-redundant storage (ZRS)
	- Geo-zone-redundant storage (GZRS)
	- Read-access geo-zone-redundant storage (RA-GZRS)
- storage account endpoint format
	- Blob Storage >> `https://<storage-account-name>.blob.core.windows.net`
	- Data Lake Storage Gen2 >> `https://<storage-account-name>.dfs.core.windows.net`
	- Azure Files >> `https://<storage-account-name>.file.core.windows.net`
	- Queue Storage >> `https://<storage-account-name>.queue.core.windows.net`
	- Table Storage >> `https://<storage-account-name>.table.core.windows.net`

### Storage Account Encryption
- all data encrypted by default (uses service-side encryption SSE)
	- no additional cost for Azure Storage encryption
	- object metadata is also encrypted
- key rotation supported
- types
	- At-rest: SSE with Microsoft-managed keys (default)
	- Customer-managed keys (CMK): use Azure Key Vault or Managed HSM
	- In transit: TLS 1.2+ / HTTPS enforced
- key management
	- Microsoft-Managed Keys (MMK): The default option where Azure handles the security, rotation, and lifecycle of your keys.
	- Customer-Managed Keys (CMK): Use Azure Key Vault or Key Vault HSM to store the key
		- gives full control over access to your encryption keys and encrypted data.
- Encryption Scopes & Infrastructure Encryption:
	- Encryption Scopes: allows encrypting individual containers or individual blobs within the same Storage Account using different Customer-Managed Keys (CMK) (ideal for multi-tenant SaaS where each customer brings their own key in Key Vault)
	- Infrastructure Encryption (Double Encryption): encrypts storage data twice (once at the service layer and once at the physical infrastructure layer) using two different encryption algorithms and two different keys for strict regulatory compliance (must be enabled at Storage Account creation or Encryption Scope creation)

## Azure storage redundancy

### Redundancy in the primary region
- data in azure storage is always replicated three times in the primary region
- primary region options are locally redundant storage LRS and zone-redundant storage ZRS
- type
	- LRS
	- ZRS
#### Locally redundant storage
- replicates data three times within a single data center in the primary region
- provides 11 nines of durability
- lowest cost option
- protects against server rack drive failures in one datacenter
- data center level disaster can cause data loss

#### Zone redundant storage
- replicates data synchronously across three Azure availability zones in primary region
- provides 12 nines of durability over a year
- data stays available for read & write operations if one zone is unavailable
- recommended for HA within a region or country

### Redundancy in a secondary region

- replicate data to a secondary region asynchronously that is geographically distant from the primary region.
- when you create a storage account, you choose the primary region.
	- azure assigns the paired secondary region based on region pairs.
- options are
	- geo-redundant storage (GRS) and
	- geo-zone-redundant storage (GZRS)
- by default, secondary-region data isn't readable unless failover occurs.
	- if the primary region is unavailable, you can fail over so the secondary region becomes primary.
	- to read secondary-region data before failover, enable read-access geo-redundant storage (RA-GRS) or read-access geo-zone-redundant storage (RA-GZRS).
- the Archive access tier for Blob Storage is not supported for ZRS, GZRS, or RA-GZRS accounts

#### Geo-redundant storage
- copies data synchronously three times in the primary region (LRS), then asynchronously to the secondary region (also LRS)
- provides at least 16 nines of durability over a year

#### Geo-zone-redundant storage
- combines zone-level resilience in the primary region with geo-replication to a secondary region
	- data copied across three AZ in primary region
	- replicated to the paired secondary region using LRS
	- provide at least 16 nines of durability over a year

### Type and redundancy

- Standard general-purpose v2
	- Supported services: Blob Storage (including Data Lake Storage Gen2), Queue Storage, Table Storage, and Azure Files
	- Redundancy options: `LRS, GRS, RA-GRS, ZRS, GZRS, RA-GZRS`
	- Usage: standard storage account type for blobs, file shares, queues, and tables; recommended for most scenarios using Azure Storage
- Premium block blobs
	- Supported services: Blob Storage (including Data Lake Storage Gen2)
	- Redundancy options: `LRS, ZRS`
	- Usage: premium SSD storage account type for block blobs and append blobs; recommended for high transaction rates, smaller objects, or consistently low storage latency
- Premium file shares
	- Supported services: Azure Files only
	- Redundancy options: `LRS, ZRS`
	- Usage: premium SSD storage account type for high-scale, high-performance enterprise file shares; supports both SMB and NFSv4.1 protocols
- Premium page blobs
	- Supported services: Page blobs only
	- Redundancy options: `LRS, ZRS`
	- Usage: premium SSD storage account type for random read/write page blob workloads and unmanaged/custom VHD disks

## Azure storage services
- Azure Blobs: A massively scalable object store for text and binary data. Also includes support for big data analytics through Data Lake Storage Gen2.
- Azure Files: Managed file shares for cloud or on-premises deployments.
- Azure Queues: A messaging store for reliable messaging between application components.
- Azure Disks: Block-level storage volumes for Azure VMs.
- Azure Tables: NoSQL table option for structured, non-relational data.


### consider when choosing storage accounts
- Storage Locations (Latency & Residency)
	- User Proximity: Place storage accounts in regions closest to users or compute to minimize latency.
	- Data Sovereignty: Align locations with regulatory mandates for physical data residency.
	- Edge Caching: Pair with Azure CDN or Front Door for fast, global delivery of public assets.
- Compliance Requirements (Retention & Auditing)
	- Immutable Storage: Apply WORM (Write-Once-Read-Many) policies for strict legal/financial preservation.
	- Legal Holds: Ensure admins can use Legal Holds to override lifecycle deletion during active audits or investigations.
	- Versioning: Enable Blob Versioning to maintain a historical audit trail and ease file recovery.
- Data Storage Costs (Tiering & Billing)
	- Cost Drivers: Pricing is shaped by redundancy settings, access tiers, and data transfer/egress.
	- Lifecycle Management: Automate transitions from Hot to Cool to Archive to slash costs.
	- Cost Allocation: Use separate accounts or an Azure Tagging Strategy to track department spend.
- Replication (Durability & Availability)
	- LRS (Locally Redundant): 3 copies in 1 datacenter. Lowest cost; best for dev/test or temporary data.
	- ZRS (Zone-Redundant): Synchronous copies across 3 availability zones. Survives datacenter outages.
	- GRS / RA-GRS (Geo-Redundant): Asynchronous secondary region copy. Survives entire regional failures.
- Administrative Overhead (Sprawl & Templates)
	- Management Burden: More accounts mean more complex security, key rotation, and RBAC upkeep.
	- IaC Automation: Define accounts as Bicep or ARM templates to prevent configuration drift.
	- GPv2 Consolidation: Group Blobs, Files, Queues, and Tables into single GPv2 accounts to minimize sprawl.
- Data Sensitivity (Network Security)
	- Network Isolation: Use Private Endpoints (Azure Private Link) to restrict access to a private VNet.
	- Secure Access: Issue short-lived, restricted Shared Access Signatures (SAS) for secure, direct client uploads.
	- Managed Identities: Authenticate app access via Entra ID Managed Identities to eliminate stored connection secrets.
- Data Isolation (Access Segregation)
	- App Boundaries: Physically isolate workloads into separate storage accounts if they have distinct regulatory bounds.

## Data Migration Options and Tools

### Azure Migrate
- unified migration platform and simplified service that manages the discovery, assessment, right-sizing, and migration of on-premises resources to Azure
- phases
	- Discover and Assess
	- Migrate and Modernise
- supported workload
	- Servers & Virtual Machines: Lift-and-shift migration of VMware, Hyper-V, and physical servers to Azure Virtual Machines
	- Databases: Assessing and migrating your SQL Server estate to Azure SQL Database, Azure SQL Managed Instance, or SQL Server on Azure VMs
	- Web Applications: Evaluating and migrating web apps to Azure App Service or Azure Kubernetes Service (AKS)
	- Virtual Desktops: Assessment and migration of on-premises Virtual Desktop Infrastructure (VDI) to Azure Virtual Desktop
	- Unstructured Data: Migrating physical file shares and objects (such as network-attached storage or NAS) to cloud file offerings like Azure Files and Azure NetApp Files, or to Azure Blob Storage
	- Bulk Data Transfer: Utilizing physical Azure Data Box appliances to copy large volumes of offline data directly into Azure Storage

### Azure Data Box
- physical migration service that helps transfer large amounts of data
- for offline bulk data migration in scenarios where network bandwidth is highly limited
- Azure Data Box Family
	- Azure Data Box Disk: Shipped as a set of up to five secure, rugged SSDs (up to 8 TB each, for a total of 40 TB per order). It is ideal for smaller-scale or recurring offline migrations.
	- Azure Data Box: A physical, rugged desktop-sized appliance with a usable capacity of approximately 80 TB. It connects directly to your local network via standard protocols (like SMB or NFS) to allow fast local copy.
	- Azure Data Box Heavy: A massive, high-capacity appliance with wheels, designed to transport up to 800 TB of usable data per unit for petabyte-scale transfers.

### AzCopy
- command line utility to copy blobs or files to or from storage account
- can upload files, download files, copy files, sync files
	- azcopy sync - one direction sync

### Azure Storage Explorer
- standalone app that provides a graphical interface to manage files and blobs in storage account
- works on windows, macOS, linux OS
- uses AzCopy on the backend for storage management tasks
- can upload, download or move between storage accounts


### Azure File Sync
- hybrid storage solution that bridges local, on-premises file servers with Microsoft's cloud-native storage infrastructure
	- synchronously replicates unstructured directories and files between your on-premises Windows Servers and fully managed cloud-native Azure Files shares
- sync groups: 
	- manage the replication relationships between your local folders and cloud files
- local caching (Cloud Tiering): 
	- transforms on premises Windows Server into a high-performance, local cache for your cloud files
	- keeps frequently accessed files ("hot" data) locally on-premises while tiering infrequently accessed files ("cold" data) to the cloud to free up local disk space.
- Compatibility with SMB & REST



## Quick Links
https://learn.microsoft.com/en-us/azure/storage/common/storage-service-encryption
