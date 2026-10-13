---
tags:
  - studynote
  - az-305
title:
author: Pralaya Panta
---


```table-of-contents
```

## Azure Virtual Machines
- when to use
	- build new workloads
	- lift and shift migration
- size
	- general purpose
	- compute optimised
	- memory optimised
	- storage optimised
	- GPU
	- high performance computes
- price includes compute costs and storage costs
- storage
	- Azure disk type
		- standard HDD
		- standard SSD
		- premium SSD
	- Managed vs unmanaged disk
- VM Operating System
	- Marketplace images vs custom images
	- Supported OS
		- Windows Server
		- Linux
		- Windows 

### Virtual Machines Scale Sets

- create and manage a group of load balanced VMs
- number of VM instances can automatically increase or decrease in response to demand or a defined schedule
- provides high availability
- no additional cost for scale set - you pay for each of VM instance created
- can also be deployed into multiple availability zones, a single availability zone, or regionally


### Azure Backup
-  backup as a service offering that protects physical or virtual machines

### Azure Site Recovery



### Azure Bastion


### Network Security Groups (NSG)


### Azure Private Links


## Azure Batch

- schedule compute-intensive tasks and dynamically adjust resources for solution without managing infrastructure
-  can create and manage a pool of compute nodes
- can also install the application you want to run, and schedule jobs to run on the compute nodes.
- Pools:
	* Reuse pools across short-running tasks to eliminate VM provisioning overhead.
	* Assign jobs to pools dynamically based on task requirements rather than using a single static pool.
- Nodes:
	* Allocate pools across multiple nodes to handle VM ephemerality and ensure steady progress.
	* Select isolated VM sizes for sensitive workloads requiring physical hardware isolation.
- Jobs:
	* Group tasks into fewer, larger jobs (such as 1 job with 1,000 tasks) to reduce scheduler overhead.
	* Use unique, structured job names to enable precise tracking and logging in Azure Monitor.


[Azure Batch](https://learn.microsoft.com/en-us/azure/batch/batch-technical-overview)