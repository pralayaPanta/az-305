---
tags:
  - studynote
  - az-305
title: Compute - Virtual Machines
author: Pralaya Panta
updated: 2026-10-23
---


```table-of-contents
```

## Azure Virtual Machines
- when to use
	- build new workloads requiring full OS control or custom kernel modules
	- lift and shift migration
- size & VM series
	- B-series (burstable) - accumulates CPU credits when idle (dev/test, low-traffic web servers)
	- D-series (general purpose) - balanced CPU-to-memory ratio (web/app servers, small-to-medium DBs)
	- F-series (compute optimised) - high CPU-to-memory ratio (batch processing, gaming, analytics)
	- E-series (memory optimised) - high memory-to-CPU ratio (SAP HANA, in-memory caches, large SQL DBs)
	- L-series (storage optimised) - high local NVMe throughput/IOPS (NoSQL DBs like Cassandra/MongoDB, data warehousing)
	- N-series (GPU) - NC/ND for AI deep learning training/inferencing; NV for remote visualization/CAD/VDI
	- H-series (high performance compute) - fastest CPUs with InfiniBand RDMA networking (fluid dynamics, financial risk modeling)
- hosting & purchasing options
	- Azure Dedicated Host - physical server dedicated to one org (hardware isolation compliance + BYOL per-core Windows/SQL licenses with Azure Hybrid Benefit)
	- Azure Spot VMs - unused capacity at up to 90% discount; evicted with 30-second notice (Deallocate vs Delete; use for interruptible batch/stateless workloads)
	- Confidential VMs - AMD SEV-SNP / Intel TDX hardware memory encryption protecting data in use
- price includes compute costs and storage costs
- storage
	- Azure Managed Disk types (replaces legacy unmanaged disks)
		- standard HDD - backup, non-critical infrequent access (95% single-VM SLA)
		- standard SSD - web servers, lightly used apps (99.5% single-VM SLA)
		- premium SSD - production DBs and latency-sensitive workloads (99.9% single-VM SLA; supports disk bursting)
		- premium SSD v2 - scale capacity, IOPS (up to 80k), and throughput (up to 1,200 MB/s) independently without downtime (supports ZRS; data disk only, not OS disk)
		- ultra disk - up to 400k IOPS, 10,000 MB/s, sub-ms latency for top-tier SAP HANA / mission-critical DBs
- VM Operating System & Images
	- Azure Compute Gallery (formerly Shared Image Gallery) - versions and replicates custom VM images across regions/subscriptions
		- generalized images - OS sysprepped / deprovisioned; prompts for machine name/admin user on deploy
		- specialized images - snapshot of an existing VM preserving machine account, profiles, and data
	- Supported OS
		- Windows Server
		- Linux
		- Windows

### High Availability, SLAs & Virtual Machine Scale Sets

- VM availability tiers & SLAs
	- single VM - 99.9% (Premium SSD / Ultra Disk), 99.5% (Standard SSD), 95.0% (Standard HDD)
	- Availability Set (99.95% SLA) - protects against rack/host failures within a single datacenter (3 Fault Domains sharing power/switch; up to 20 Update Domains for planned reboots)
	- Availability Zones (99.99% SLA) - 2+ VMs deployed across 2+ physically separate datacenters in a region (independent power, cooling, networking)
- Virtual Machine Scale Sets (VMSS)
	- create and manage a group of load balanced VMs
	- number of VM instances can automatically increase or decrease in response to demand or a defined schedule
	- Flexible Orchestration (recommended) - distributes up to 1,000 VMs across Availability Zones and Fault Domains, mixes Spot + Pay-As-You-Go VMs, and supports standard individual VM operations
	- no additional cost for scale set - you pay for each of VM instance created
- Proximity Placement Groups (PPGs)
	- logical grouping forcing VMs, VMSS, or Availability Sets into the same physical datacenter spine for ultra-low network latency (e.g., SAP app tier + DB tier; combine with Accelerated Networking)
- Composite SLA calculation
	- sequential tiers (Web AND DB) - multiply SLAs (99.9% x 99.99% = 99.89%)
	- redundant active-active instances - 1 - (Unavailability_A x Unavailability_B)


### Azure Backup
- backup as a service offering that protects physical or virtual machines via Recovery Services Vault (Standard daily vs Enhanced 4-hr policy, Instant Restore snapshots, File Recovery, Cross-Zone/Cross-Region Restore)

### Azure Site Recovery
- replicates Azure VMs across regions (via Mobility Service + Cache Storage Account, 5-min crash-consistent RPO, zero target VM compute cost until failover) and orchestrates failover via Recovery Plans

### Azure Bastion
- fully managed PaaS jump-host deployed in AzureBastionSubnet (/26 or larger) providing secure RDP and SSH over TLS (Port 443) via the Azure Portal or native client without exposing VM Public IPs

### Network Security Groups (NSG)
- stateful Layer-4 packet filter attached to Subnets and/or VM NICs evaluating 5-tuple rules by priority (100-4096); pair with Application Security Groups (ASGs) to group VMs logically instead of hardcoding IPs

### Azure Private Links
- Azure Private Endpoint assigns a private IP from your VNet to a PaaS resource (Storage, SQL, Key Vault) so VMs access it privately over the Microsoft backbone via privatelink Private DNS Zones


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