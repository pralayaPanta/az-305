---
tags:
  - studynote
  - az-305
title: Storage - non relational data
author: Pralaya Panta
---


```table-of-contents
```

## Choosing data storage

- **Consider Azure Blob Storage**. 
	- Store vast amounts of unstructured data by using Azure Blob Storage. 
	- Blob Storage is often used for images and multimedia files.
    
- **Consider Azure Files**. 
	- Provide fully managed file shares in the cloud with Azure Files. 
	- This storage data is accessible via the industry standard Server Message Block (SMB) protocol, Network File System (NFS) protocol, and the Azure Files REST API.
    
- **Consider Azure managed disks**. 
	- Support Azure virtual machines by using Azure managed disks. 
	- These disks are block-level storage volumes managed by Azure. 
	- Managed disks perform like physical disks in an on-premises server, but in a virtual environment.
    
- **Consider Azure Queue Storage**. 
	- Use Azure Queue Storage to store large numbers of messages. 
	- Queue Storage is commonly used to create a backlog of work to process asynchronously.


## Azure Blob Storage
- blob stands for Binary Large Object. 
- consider while implementing
	- blob access tier
	- immutable storage need

### blob access tier
1. Hot
	- availability 3 nines & 4 nines (RA-GRS)
	- latency time to first byte >> ms
	- optimised for frequent reads & write
	- by default new storage accounts are created in hot tier
	- lowest access costs, but higher storage costs
2. Cool
	-  availability 2 nines & 3 nines (RA-GRS)
	-  latency time to first byte >> ms
	- optimised for storing large amounts of infrequently used data
	- minimum storage duration is 30 days
	- use case - short term backup & disaster recovery datasets or older media
	- higher access costs than hot tier; lower storage costs
3. Cold
	-  availability 2 nines & 3 nines (RA-GRS)
	-  latency time to first byte >> ms
	- optimised for storing large amounts of infrequently used data
	- intended for data that can remain in for at least 90 days	
4. Archive
	- availability 2 nines & 3 nines (RA-GRS)
	- latency time to first byte >> hours
	- offline tier for data that can tolerate several hours of retrieval latency
	- data must remain in the tier for at least 180 days or be subject to an early deletion charge
	- includes secondary backups, original raw data, and legally required compliance information
	- most cost-effective option for storing data
	- accessing data is more expensive than any tier

### immutable storage
- enables users to store business-critical data in a WORM (Write Once, Read Many) state
	- in WORM state data cannot be modified or deleted for a user specified interval
	- protects from overwrites and deletion
	- applied at container level with audit logs
- no extra capacity charge for using immutable storage
	- creating or deleting a time-based retention policy or legal hold on a blob version results in a write transaction charge. 
- immutability policies
	- time based retention policies
		- objects can be created and read, but not modified or deleted for that time
		- <u> after the retention period expires, objects can be deleted, but not overwritten</u>
		- Hot, Cool, and Archive access tiers support immutable storage by using time-retention policies.
		- min retention period - 1 day; max retention period - 400 yrs
		- effective retention policy
			- $\text{Retention Exp Date} = \text{Blob Creation Time} + \text{User-Specified Retention Interval}$
		- locked versus unlocked policies
			- by default the policy is created in unlocked state for testing purposes
			- can lock the policy so that it's fully compliant 
			- both locked and unlocked policies protect against deletes and overwrites
			- unlocked policy
				- can modify an unlocked policy by shortening or extending the retention period
				-  can also delete
			- locked policy
				- can't delete a locked time-based retention policy
				- can extend the retention period, but you can't decrease it
				- max five increases to the effective retention period is allowed over the lifetime of a locked policy at container level
	- legal hold policies
		- store immutable data until the legal hold is explicitly cleared
		- objects can be created and read, but not modified or deleted
		- Premium Blob Storage uses legal holds to support immutable storage
- features WORM
	- container level
		-  allows to set policies at the container level only
		-  can't configure individual blobs with their own immutability policies
	- version level
		-  allows to set policies at the account, container, or version level
		- **If there's a legal hold on a container,** 
			- you can't create version-level WORM for the same container
			- as legal hold prevents generating the versions
		- If using version-level WORM, the bill might be higher because you enabled versioning, and there's a cost associated with extra versions being stored.

- immutable storage with blob soft delete
	- applies to all blobs within the account regardless of whether a legal hold or time-based retention policy is in effect
	- enable soft delete for extra protection before any immutability policies are applied
	- If you enable blob soft delete and then configure an immutability policy, any blobs that you already soft deleted are permanently deleted once the soft delete retention policy expires.

## Azure Files
- files are accessible by using the industry standard Server Message Block (SMB) protocol, Network File System (NFS) protocol, and the Azure Files REST API.
- can mount or connect to an Azure file share at the same time on all the main operating systems.
- can be used to add to or replace a company's existing on-premises network attached storage (NAS) devices or file servers.
- data is encrypted in transit by using HTTPS and is stored encrypted when at rest.
- data access method
	- direct mount of Azure file shares
	- cache Azure file shares on-premises with Azure File Sync
- performance level
	- standard performance accounts - backed by HDD
	- premium storage account - backed by SSD
	- SSD file shares support LRS and ZRS redundancy
	- Geo-redundant options (GRS and GZRS) are only available for HDD file shares
- storage tier
	- premium
		- SSD backed for high performance & low latency
		- for most intensive IO workloads
		- can be used for SMB and NFS protocol
	- transaction optimised
		- standard storage hw - backed by HDDs
		- for heavy workload that do not need latency offered by premium file shares
	- hot access tier - backed by HDDs
	- cool access tier
		- cost efficient storage for online archive
		- backed by HDDs
- billing model
	- provisioned v2 (recommended)
	- pay-as-you-go
	- provisioned v1 (legacy)

## Azure managed disks
- used by virtual machines to store data like database files, website static content, or custom application code.
- types of managed disk
	- Ultra Disk
		- best for: Top-tier databases (SQL Server, Oracle, SAP HANA) and massive, high-transaction OLTP systems
		- Offers the highest throughput and IOPS in Azure
		- can dynamically adjust performance settings on the fly without stopping the associated VM
		- might not be available in all regions.
	- Premium SSD v2
		- best for: Production enterprise workloads that require high performance but have variable capacity-to-performance ratios
		- lets you separately provision disk size, IOPS, and throughput required 
		- higher performance & cost effective than Premium SSD
		-  isn't available in all regions
	- Premium SSD
		- best for: production and performance-sensitive virtual machines.
		- performance limits (IOPS and throughput) are strictly tied to the overall physical size of the provisioned disk
		- available in all regions.
	- Standard SSD
		- Web servers, lightly used enterprise applications, and non-production (Dev/Test) env
		- cost-effective SSD tier 
	- Standard HDD
		- best for: Backups, non-critical staging workloads, and infrequently accessed file directories
		- backed by traditional magnetic hard disk drives (HDDs); lowest performance tier, but highly cost-efficient for heavy, sequential read/write operations.

- encryption option
	- azure disk encryption ADE
		- encrypts VM virtual hard disk VHD
		- if VHD is protected with ADE the disk image is accessible only by the VM that owns it
	- server side encryption SSE
		- performed on physical disk in the data centre
		- known as encryption at rest or azure storage encryption
	- encryption at host
		- ensures data stored in VM host is encrypted at rest
		- are not encrypted with SSE
		- the server hosting VM provides encryption and encrypted data flows into azure storage
- data caching
	- optimizes read and write access to the virtual hard disk (VHD) files
	- for OS disk default cache settings is ReadWrite
	- for data disk default cache settings is ReadOnly
	- is not supported for disks larger than 4TiB
	- changing the cache settings of azure disk detaches & reattaches the target disk
		- if OS disk the VM is restarted

## Azure NetApp
- support for both SMB and NFS protocols, built-in data deduplication and compression, and high-performance storage capabilities.


## Storage Security

- Shared Access Signatures (SAS)
	- provides temporary, secure, and granular control over client access to specific resources without revealing your account keys
- Firewalls & Network Rules
	- Protects the public endpoint by restricting access to trusted IP ranges, individual IPs, or specified VNet subnets. 
- Secure Transfer (HTTPS)
	- Automatically rejects any non-secure (HTTP) connections. Microsoft recommends keeping this setting enabled for all storage accounts.
	- The `Secure transfer required` property is enabled by default when an Azure storage account is created
- Consider private endpoints
	- a special network interface for an Azure service
	-  provides secure connectivity between clients on your virtual network and your storage.

## Quick Link
https://learn.microsoft.com/en-us/azure/storage/blobs/immutable-storage-overview
https://learn.microsoft.com/en-us/azure/storage/files/storage-files-introduction
https://learn.microsoft.com/en-us/azure/virtual-machines/disks-types