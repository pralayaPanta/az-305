---
tags:
  - studynote
  - az-305
title:
author: Pralaya Panta
---

```table-of-contents
```



## Azure Backup

- backup type supported
	- on prem
		- using MARS agent
	- azure VM
		- backup extension 
	- azure files
	- SQL Server in Azure VM
	- SAP HANA databases in Azure VM
- storage vaults
	- azure backup vaults
		- used by Azure backups only
	- Recovery Services Vault
		- used with Azure Backup or Azure Site Recovery
- azure policy can be used to propagate backup policy across multiple vaults
- can protect against ransomware by making backup vault immutable

### Azure Blob Storage backup and recovery

- backup data is stored in  source Azure storage account rather than being transferred to an Azure Backup storage vault
- soft delete feature
	- protect data from accidental deletion or corruption
	- available for blobs and containers
	- retention period for deleted blobs or containers can be specified between 1-365 days
		- default is 7
	- can restore a soft deleted object to its state at the time it was deleted
- blob versioning
	- restore an earlier version of blob or recover data after incorrect data modification or deletion
	- useful when you have multiple authors editing the same files
- point in time restore feature for block blobs
- resource lock
	- prevents resources from being accidentally deleted or changed
	- CanNotDelete tag - only read and modify not delete
	- ReadOnly - only read resource, not deleted or change


### Azure files backup and recovery


- share snapshots of file shares
	- capture share state at that specific point in time
	- can be created manually
	- can be automated by using Azure Backup and backup policies
	- happens at root level of files shares - includes all files and folder
	- snapshots are incremental
	- cannot delete a share with snapshot
	- can configure snapshot backups for daily, weekly, monthly, or yearly retention.
- automated file share backups
	- vaulted backup
		- stores backup in a backup vault - offsite copy
		- support retention up to 10 yrs
		- protection against deletion and ransomware
		- vaulted backup for comprehensive protection
	- snapshot backup
		- stored locally with in storage account 
		- cannot protect against account level deletion

### Azure virtual machine backup and recovery


- allows for simple configuration and scaling for Windows and Linux virtual machines
- policy types
	- standard policy
		- once daily backups
		- does not support trusted launch VMs or Ultra Disk or Prem SSD v2
	- enhanced policy
		- upto every 4 hours
		- for trusted launch VMs
- phases
	- VM snapshot is taken
	- snapshot transferred to a RSV
- supports application, system and crash consistent snapshot backups

### Azure SQL backup and recovery


- automated backups
	- full backups
		- once a week
	- differential backups
		- changed since last full backup
		- every 12-24 hours
	- transactional backups
		- transaction logs content are backed up
		- every 10 min
- automatic backups remain available to restore for up to 35 days
- long-term retention (LTR) feature
	- can store backup in blob storage for up to 10 years


### Azure Site Recovery

- disaster recovery of  Azure virtual machines, and fail over from a primary region to a secondary region.
- replicate on-premises virtual machines and physical servers to Azure, or to a secondary on-premises datacenter


[Azure Site Recovery](https://learn.microsoft.com/en-us/azure/site-recovery/site-recovery-overview)