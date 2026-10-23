---
tags:
  - studynote
  - az-305
title: Migration - Design and Tools
author: Pralaya Panta
updated: 2026-10-23
---


```table-of-contents
```


## Cloud Adoption Framework methodologies

- Define strategy
- Plan
- Ready
- Adopt
	- migrate and innovate
- Govern
- Manage

## Azure migration framework

- Assess
- Migrate
- Optimize 
- Monitor

#### Migration strategy patterns

- Rehost
	- lift and shift migration
	- no code changes 

- Refactor
	- repackaging 
	- minimal changes to apps 
- Rearchitect
	- modifying and extending app functionality and code base
	- breaking down monolithic app into a group of microservices
- Rebuild
	- rebuilding an app using cloud technologies
- Replace
	- retire custom on-premises app in favor of a commercial SaaS solution (e.g., Microsoft 365, Salesforce)



## Tools


- Azure Migrate
	- performs assessment and migration to Azure of virtual machines (Hyper-V and VMware), cloud-based virtual machines, physical servers, databases, data, virtual desktop infrastructure, and web applications
	- agentless environment discovery or use agents to perform a dependency analysis
	- makes recommendations for the size of virtual machine to create
		- Performance-based sizing (recommended) - analyzes historical CPU, RAM, disk IOPS, and network utilization to right-size Azure VMs
		- As-is on-premises sizing - maps exact on-premises core and RAM counts 1:1 without considering actual utilization
	- server migration modes
		- Agentless migration - supported for VMware vSphere via vCenter APIs (up to 500 VMs concurrently) and Hyper-V
		- Agent-based migration - uses Mobility Service + Replication Appliance (required for physical servers, AWS EC2, GCP VMs)
	- supports the visualization of dependencies
	- tools
		- Azure Migrate: Discovery and assessment
		- Azure Migrate: Migration and modernization
		- Azure Migrate: App Containerization (packages existing ASP.NET and Java web apps into containers for AKS or Azure Container Apps)
		- App Service Migration Assistant
		- Azure SQL Migration extension for Azure Data Studio (replaces standalone SQL Server Data Migration Assistant / DMA)
		- Application and Code Assessment for .NET and Java
		- Azure Database Migration Service
		- Azure Data Box
[Azure Migrate documentation](https://learn.microsoft.com/en-us/azure/migrate/migrate-overview)

- Database Migration Service
	- performs assessment and migration for several different databases, not just Azure SQL Database.

- Data Migration tool
	- The Azure Cosmos DB Data Migration tool migrates your existing databases to Azure Cosmos DB.

- Azure Monitor 
	- collects monitoring data from both on-premises and Azure resources that help you analyse data, set up alerts, and identify problems.

- Azure Advisor
	- helps optimize your Azure resources for reliability, performance, cost, security, and operational excellence.


### Azure Migrate Discovery and assessment tool

1. Download and configure the appliance.
2. Start discovery.
3. Create an assessment.
4. Review the assessment.


### Azure Resource Mover

- move Azure resources between subscriptions, resource groups, and regions
- automatic clean-up of resources in the source region
- the ability to test a move operation before committing


### Azure Database Migration Service

- used for
	- Azure SQL Database
	- Azure SQL Managed Instance
	- Azure SQL VM
	- Azure Cosmos DB
	- Azure Database for MySQL
	- Azure Database for PostgreSQL
- types
	- offline migration 
		- requires shutting down the server at the start of migration
	- online migration
		- continuous sync of live data 
		- minimises downtime
- migration scenario
	- SQL Server to Azure SQL Managed Instance - online or offline
		- Managed Instance Link - creates a Distributed Availability Group (DAG) between SQL Server and SQL MI for near-zero-downtime cutover and bidirectional failback (SQL Server 2022+)
		- Log Replay Service (LRS) - continuously replays transaction log backups (.trn) from Azure Blob Storage into SQL MI until cutover
	- SQL Server to SQL Server on Azure Virtual Machine - online or offline
	- SQL Server to Azure SQL Database - offline (BACPAC export/import) or online (DMS / transactional replication)
- Azure Database Migration Service can be automated by using PowerShell
[the Azure Database Migration Service](https://learn.microsoft.com/en-us/azure/dms/dms-overview)

### Azure Storage Mover

- fully managed service to migrate files and folder to Azure storage
- work on both NFS shares and SMB shares
	- can migrate SMB 2.x protocol source endpoints
		- file and folder timestamps, ACLs, file attributes are maintained
	- can migrate NFS 3.x and 4.x source endpoints
		- metadata is persisted in custom metadata field of blob
- for initial bulk migration can be used with Azure Data Box and Storage Mover for online catch up
[a server with the Azure Storage Migration Service](https://learn.microsoft.com/en-us/windows-server/storage/storage-migration-service/migrate-data)
### Azure File Sync

- feature of Azure Files
- can be used to centralise file shares management
- can be also used to cache Azure File shares on windows server computer for fast access
- can use protocol like SMB< NFS< FTPS to access files locally
- scenarios
	- replace or supplement on prem file servers
	- lift and shift
	- backup and disaster recovery
	- azure file sync



### Azure Import/Export service

- migrates large quantities of data between an on-premises location and an Azure storage account
- send and receive physical disks that contains data 
- disk drives can be Serial ATA (SATA) hard-disk drives (HDDs) or solid-state drives (SSDs)
- can be used to export data from Azure Blob storage
- cannot export data store in Azure Files
- need set of disks to be sent to datacentre
- for import job, 
	- write  data to these disks and ship them to an Azure datacenter
	- uploaded by Microsoft


[Azure Import/Export](https://learn.microsoft.com/en-us/azure/import-export/storage-import-export-service)



### Azure Data Box

- quick, reliable, and inexpensive method for moving large volumes of data to Azure
- transferring data sizes larger than 40 TB or more
- secure, tamper-proof, single hardware appliance
- components
	- Data Box device
		- physical device with primary storage
		- two config
			- 120 TB capacity - for transfers for 40-480 TB
			- 525 TB capacity - for same transfer
	- Data Box service
		- extension in portal
		- perform daily administration of the device
		- create and mange orders, view, alerts and manage shares
	- Data Box local web based user interface
		- web based UI to configure the device 
		- can be used to shutdown and restart the device
		- can be used to view and copy logs


[Azure Data Box](https://learn.microsoft.com/en-us/azure/databox/)

[the Microsoft Cloud Adoption Framework for Azure](https://learn.microsoft.com/en-us/azure/cloud-adoption-framework/overview)

[on-premises workloads for migration to Azure](https://learn.microsoft.com/en-us/training/modules/prepare-onpremises-workloads-migration-azure/)