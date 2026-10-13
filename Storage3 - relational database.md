---
tags:
  - studynote
  - az-305
title: Storage - relational database
author: Pralaya Panta
updated: 2026-09-13
---

```table-of-contents
```



# PaaS options for SQL Server in Azure

- Azure SQL Database
- Azure SQL Managed Instance

## Azure SQL Database

- PaaS deployment option of Azure SQL that abstracts both the OS and the SQL Server instance
- fully managed service
	- don't have to deal with database tasks like configuring and managing high availability, tuning, and backups.
- only deployment option that supports scenarios that require large databases (currently up to 128 TB) or autoscaling for unpredictable workloads (serverless)
- pricing options
	- vCore - virtual core
		- choose number of virtual cores
		- supports azure hybrid benefit for SQL server and reserved capacity
		- tiers
			- general purpose
			- business critical
			- hyperscale
	- DTU - database transaction unit
		- combined measure of compute, storage and IO resources
		- easy and preconfigured purchase option
		- tiers 
			- basic 
			- standard 
			- premium
	- Serverless
		- available for general purpose & hyperscale db
		- automatically scales compute
		- workload no longer requires compute resources, the database is paused
			- only storage is billed
			- upon connection database resumes
			- use setting called autopause delay (between 60min to 7 days)
- can deploy
	- single database
		- single db managed per db level 
		- have dedicated resources even if deployed to the same logical server
	- elastic pool
		- group of db manage and share common set of resource
		- cost effective solution 
- when to use 
	- cloud native and modern app development
	- fully managed db without managing logical servers or instance level config

### Elastic Pools
- basic
	- auto scale up to 5 eDTUs per DB
- standard
	- auto scale up to 100 eDTUs per DB
- premium
	- auto scale up to 1000 eDTUs per DB



## Azure SQL Managed Instance
- PaaS deployment option providing instance of SQL server
- can deploy single managed instance or a managed instance pool
	- single managed instance
		- SQL server surface area
		- native virtual network support
		- fully managed service
	- instance pool
		- resource sharing between multiple instance to optimise cost
		- fully managed service
- can use managed instance to do <u>**lift shift migration**</u>
- can be used where instance scoped feature is ideal
	- SQL server agent
	- common language runtime CLR
	- db mail, distributed transactions & ML services
- uses vCores mode
	- can define max CPU cores
	- can define max storage allocated
- upgrade to next-gen general purpose for more capacity
	- 500 db per instance
	- 32 TB storage
	- scale resources independently

## Database and dynamic scalability
- change resources allocated such as CPU power, memory, I/O throughput, and storage with minimal downtime
- dynamic scalability for an Azure SQL database
	- choose DTU or vCore models
	- use elastic database pools 
	- implement vertical or horizontal scaling
- vertical scaling - scaling up
	- increase or decrease the compute size of database 
	- using SQL database elastic database pools
- horizontal scaling - scaling out
	- add or remove databases to adjust capacity
	- using sharding to partition data or read scale-out provisioning
- sharding
	- splits data horizontally across multiple databases for independent scaling
	- distributes the data & compute workload across multiple distinct database server
	- shard map manager helps app find the right database using the sharding key
		- **Elastic Database Client Library** - Shard Map Manager

- example scenario
	- manage & scale multiple azure SQL database that have varying & predictable resource requirements - **SQL elastic pools**
	- developing new app with single db to test before launching - **SQL Database or Managed Instance**
	- optimise the price performance for a group of databases within a prescribed budget while delivering performance elasticity for each database - **SQL elastic pools**
	- different sections of a database reside in different geographic locations for compliance reasons - **horizontal scaling and sharding**


## High Availability

- tier
	- general purpose / standard tier
	- business critical / premium tier
	- hyperscale tier
- general purpose / standard tier
	- common workloads
	- cost optimised for compute and storage
	- uses node with spare capacity to spin up a new SQL server instances
	- uses LRS and RA-GRS (backup files)
- business critical / premium tier
	- designed for OLTP app
	- high transaction rate and low IO latency
	- deploys an Always On Availability group using multiple synchronously updated replicas
	- uses local SSD storage
	- RA-GRS (backup files)
- hyperscale tier
	- designed for very large OLTP databases as large as 100TB
	- auto scale storage & scale
	- captures instantaneous backups using snapshots
	- restores in minutes rather than hours & days
	- scale up or down in real time 

## Active geo-replication and Failover groups

- active geo-replication
	- Azure SQL Database only
	- asynchronously replicates a database to up to four secondary replicas
	- as transactions are committed to the primary (and its replicas within the same region), the transactions are sent to the secondaries to be replayed
	- failover
		- can be done manual or programmatically from app
		- in case of failover connection strings need to be updated

- failover groups
	- Azure SQL Database or Azure SQL Managed Instance
	- built on top of the technology used by geo-replication
	- provides single endpoint for connection
	- failover without connection string changes


# SQL Server on Azure Virtual Machines 
- runs on an Azure VM
	- use full version of SQL server
	- access to the full capabilities of SQL server
	- you are responsible for update operations for OS and SQL Server
- Transaction Coordinator (DTC), and Simple Recovery model
	- SQL Server Integration Services (SSIS)
	- SQL Server Reporting Services (SSRS) and
	- SQL Server Analysis Services (SSAS)


### SQL Server HADR features


- Always On Failover Cluster Instance (FCI)
	- instance level protection 
	- single copy of databases stored on shared storage
	- needs to be configured when database is first installed
	- requires
		- Windows Server Failover Cluster or Pacemaker
		- AD DS, DNS, Internal LB in Azure
	- failover process
		- full service stop in failing node
		- starts up on the standby node
		- existing client connections dropped
		- during startup on the new node, databases go through recovery


- Always On Availability Groups (AG)
	- database level protection
	- requires
		- Windows Server Failover Cluster or Pacemaker
		- AG listener
	- no shared storage required - independent copy of database
	- failover process
		- faster failover than an FCI
		- database role switches from secondary to primary


- Log Shipping
	- database protection
	- does not require Windows Server Failover Cluster or Pacemaker
	- no native listener 
	- process
		- takes full backup of primary db and restores in secondary server using norecovery or standby nmode
		- agent jobs automatically copy transaction log 
		- application must change the connection string or network level DNS alias change


# Database Security and recovery

- data discovery, classification, and protection
- transparent data encryption (TDE)
	- protects Azure SQL Database, Azure SQL Managed Instance, and Azure Synapse Analytics against the threat of malicious offline activity by encrypting data at rest.
	- performs real time encryption and decryption of the database, backups, log files at rest
	- enabled by default to all newly deployed azure SQL database
- Encryption
	- data at rest
		- always encrypted
		- transparent data encryption TDE
	- data in motion
		- always encrypted
		- transport layer security TLS 1.2 or higher
	- data in process
		- dynamic data masking
		- specific data is unencrypted; remaining data is encrypted



## Backup and recovery for databases

- Database Backups are taken on a given schedule:
	- **Full –** Once a week
	- **Differential –** Every 12 hours
	- **Log –** Every 5-10 minutes depending on transaction log activity


- long term retention policy
	- automatic backups remain available for up to 35 days
	- long-term retention (LTR)
	- store Azure SQL database backups in Azure Blob for up to 10 years








https://learn.microsoft.com/en-us/azure/azure-sql/database/?view=azuresql
https://learn.microsoft.com/en-us/azure/azure-sql/managed-instance/?view=azuresql
https://learn.microsoft.com/en-us/azure/azure-sql/virtual-machines/?view=azuresql