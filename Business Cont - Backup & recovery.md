---
tags:
  - studynote
  - az-305
title: Business Continuity - Backup & Recovery
author: Pralaya Panta
updated: 2026-10-23
---

```table-of-contents
```



## Azure Backup

- backup type supported
	- on prem
		- using MARS agent (files/folders/system state to RSV, up to 3x/day)
		- using MABS / System Center DPM (app-aware VMware/Hyper-V/SQL/SharePoint/Exchange backups)
	- azure VM
		- backup extension 
	- azure files
	- SQL Server in Azure VM
	- SAP HANA databases in Azure VM
- storage vaults
	- Backup Vault (newer workloads)
		- supports Managed Disks, Azure Blobs, PostgreSQL Flexible Server, AKS, and Azure NetApp Files
	- Recovery Services Vault (RSV)
		- supports Azure VMs, SQL/SAP HANA in Azure VMs, Azure Files, MARS/MABS/DPM, and Azure Site Recovery (ASR)
- vault storage redundancy (configured before registering first item)
	- LRS (lowest cost, single datacenter), ZRS (across Availability Zones), GRS (default, paired region)
	- Cross-Region Restore (CRR): requires GRS (RA-GRS); allows restoring backup data into the paired secondary region at any time (for compliance drills or regional outages)
- vault security & protection
	- azure policy can be used to propagate backup policy across multiple vaults (`DeployIfNotExists`)
	- Soft Delete: retains deleted backup data for 14–180 days (14 days free by default; can be locked)
	- Immutable Vault (WORM): blocks deleting recovery points or lowering retention against ransomware (can be locked irreversibly)
	- Multi-User Authorization (MUA) via Resource Guard: requires approval from a separate Resource Guard in another subscription/tenant before critical vault operations (disabling soft delete, modifying policies, deleting backups)

### Azure Blob Storage backup and recovery

- two backup modes via Backup Vault:
	- Operational Backup: continuous point-in-time backup stored locally in the source storage account (uses Blob Versioning, Change Feed, Soft Delete; up to 360 days; fast local restore)
	- Vaulted Backup: periodic daily snapshots transferred offsite into Backup Vault (retained up to 10 years; protects against storage account deletion or ransomware)
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
- Customer-Managed Storage Account Failover:
	- manually fails over `GRS / RA-GRS / GZRS / RA-GZRS` accounts to the secondary region (`<15 min` RPO async data loss risk)
	- Gotcha: after failover, the account becomes LRS in the new primary region — you must manually re-enable GRS / GZRS
- resource lock
	- prevents resources from being accidentally deleted or changed
	- `CanNotDelete` lock - only read and modify, not delete
	- `ReadOnly` lock - only read resource, not delete or change


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
		- stored locally within storage account
		- cannot protect against account level deletion

### Azure virtual machine, disk & AKS backup and recovery


- allows for simple configuration and scaling for Windows and Linux virtual machines
- policy types
	- standard policy
		- once daily backups
		- does not support trusted launch VMs or Ultra Disk or Prem SSD v2
	- enhanced policy
		- up to every 4 hours (instant restore snapshots kept up to 30 days)
		- required for Trusted Launch VMs, Ultra Disks, and Premium SSD v2
- phases
	- Phase 1: instant restore snapshot taken locally alongside disks (1–5 days default for fast restore)
	- Phase 2: snapshot transferred to Recovery Services Vault (vault-standard tier; optional vault-archive tier for LTR)
- supports application-consistent (VSS), file-system-consistent (Linux pre/post scripts), and crash-consistent backups
- VM Restore options:
	- Create New VM, Replace Existing Disks, or Restore Disks (to build a custom VM)
	- File Recovery: mounts recovery point via iSCSI script to copy individual files/folders without full VM restore
	- Cross-Zone Restore (CZR) (for ZRS vaults) and Cross-Region Restore (CRR) (for GRS/RA-GRS vaults)
- Azure Managed Disk Incremental Snapshots:
	- billed only for delta block changes since the last snapshot; stored on Standard HDD and copyable across regions
- Azure Kubernetes Service (AKS) Backup:
	- uses Backup Vault + AKS Backup Extension (plus staging Blob storage + snapshot RG) to back up both cluster state/YAML manifests and Persistent Volumes


### Database backup and recovery


- Azure SQL Database / SQL Managed Instance automated backups:
	- full backups: once a week
	- differential backups: changed since last full backup (every 12-24 hours)
	- transactional log backups: every 10 min
	- point-in-time restore (PITR) retention: 1–35 days (default 7 days; stored on RA-GRS by default, configurable to LRS/ZRS)
	- Long-Term Retention (LTR): stores full backups for up to 10 years
- SQL Server on Azure VMs (IaaS) backup options:
	- Automated Backup (SQL IaaS Extension): simple backup to Blob Storage (up to 90 days retention)
	- Recovery Services Vault SQL Backup (Enterprise recommended): 15-minute RPO (log backups every 15m), Auto-Protection for new DBs on the instance, and up to 10-year LTR
- Azure Cosmos DB backup modes:
	- Continuous Backup (PITR): restores to any second within the last 7 days or 30 days
	- Periodic Backup: snapshots taken every 1–24 hours (default 4h), retained 8 hours to 30 days (2 free copies)


### Azure Site Recovery

- disaster recovery of Azure virtual machines (Region-to-Region), and replication of on-premises VMware, Hyper-V, and physical servers to Azure
- Azure-to-Azure DR architecture:
	- Mobility Service extension (inside source VM) streams disk writes -> Cache Storage Account (source region) -> Target Managed Disks (target region)
	- No target VM compute cost until failover occurs (only pay for ASR license + target disk storage)
	- RPO: crash-consistent recovery points every 5 minutes; app-consistent every 1–12 hours
- Recovery Plans:
	- orders multi-tier app failover using numbered Boot Groups (e.g., Group 1: DB -> Group 2: App -> Group 3: Web)
	- supports Azure Automation runbooks (update DNS, attach Public IPs/Load Balancers) and manual steps
- Failover types & lifecycle:
	- Test Failover: boots temporary VMs in an isolated non-production VNet with zero impact on production or replication
	- Planned Failover: clean shutdown of source VMs with zero data loss
	- Unplanned Failover: boots target VMs from latest recovery point when primary region is down
	- Commit -> Reprotect -> Failback: after failover, click Commit, then Reprotect to reverse replication back to the primary region


[Azure Site Recovery](https://learn.microsoft.com/en-us/azure/site-recovery/site-recovery-overview)