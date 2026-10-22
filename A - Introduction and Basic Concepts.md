---
tags:
  - studynote
  - az-305
title:
author: Pralaya Panta
---




```table-of-contents
```


## High Availability

### RTO vs RPO

- Recovery Time Objective (RTO)
	- max. amount of time available to bring resources online after an outage or problem

- Recovery Point Objective (RPO)
	- point in time to which database should be recovered ad equates to max amount of data loss that business is willing to accept

### Availability Sets
- provide uptime against azure related maintenance and single point of failure
- designed to enhance the reliability and availability of virtual machines by separating them into 
	- fault domains (FD)
		- groups of servers within a data center that share the same power source and network
		- each data center can have up to three fault domains
	- update domains (UD)
		- represent groups of virtual machines and their underlying physical hardware that can be rebooted simultaneously

### Availability Zones
- account for data center-level failure in Azure
- a zone is a unique physical location, that is, a data center, within an Azure region
- each zone have separate network, power


### Azure Site Recovery
- replicates a VM from one Azure region to another to create a disaster recovery solution for that VM
- may meet RTO, it may not meet RPO since it isn't accounting for where data is inside SQL Server

### SQL Server HADR Features for Azure Virtual Machine

- Always On Failover Cluster Instance (FCI)
	- Provides instance-level protection by restarting the entire SQL Server instance on an alternate cluster node during a failover
- Always On Availability Group (AG)
	- Provides **database-level** protection
	- Uses an **AG Listener** (IP and virtual name) for client connectivity abstraction.
	- Non-database objects (e.g., SQL Agent jobs, instance-level logins, linked servers) are not captured in the transaction log and must be manually synced across nodes
- Log Shipping
	- based on backup, copy, and restore,
	- lacks native connection abstraction, requiring clients to handle server name changes or use network-level DNS aliases


## Region Pairs

- paired with another region within the same geography
- approach allows for the replication of resources across a geography


## Sovereign Regions

- US DoD Central, US Gov Virginia, US Gov Arizona
	- physical and logical network-isolated instances of Azure for U.S. government agencies and partners
- China East, China North, and more
	-  unique partnership between Microsoft and 21Vianet
## Cloud Hosting
- **IaaS (Infrastructure as a Service)**:
    - Consumer manages most (OS, apps, security).
- **PaaS (Platform as a Service)**:
    - Shared management (apps and data by consumer, platform by provider).
- **SaaS (Software as a Service)**:
    - Provider manages almost everything.
    - Consumer focuses on data and access.

[Shared Responsibility Chart](https://learn.microsoft.com/en-us/azure/security/fundamentals/shared-responsibility)


## Azure Compute Intro

- **Azure Virtual Machines**: 
	- Deploy and manage virtual machines inside an Azure virtual network
- **Azure Batch**: 
	- Apply this managed service to run large-scale parallel and high-performance computing (HPC) applications
- **Azure App Service**: 
	- Host web apps, mobile app backends, RESTful APIs, or automated business processes with this managed service
- **Azure Functions**: 
	- Use this managed service to run code in the cloud, without worrying about the infrastructure
- **Azure Logic Apps**: 
	- Configure this cloud-based _platform_ to create and run automated workflows similar to capabilities in Azure Functions
- **Azure Container Instances (ACI)**: 
	- Run containers in Azure in a fast and simple manner without creating virtual machines or relying on a higher-level service
- **Azure Container Apps (ACA)**: 
	- Run containerized applications on a fully managed serverless platform without managing Kubernetes infrastructure. 
	- Supports scale-to-zero, Dapr integration, jobs, and revision-based traffic management
- **Azure Kubernetes Service (AKS)**: 
	- Run containerized applications with this managed Kubernetes service.


[computer decision tree flowchart](https://learn.microsoft.com/en-us/azure/architecture/guide/technology-choices/compute-decision-tree)





### Azure hierarchy

- **Management groups** help you manage access, policy, and compliance for multiple subscriptions.
	- can be used to aggregate policy and initiative assignments via Azure Policy
	- can support up to six levels of depth
	- by default, all new subscriptions are placed under the root management group
    
- **Subscriptions** are logical containers that serve as units of management and scale. Subscriptions are also billing boundaries.
	- Subscriptions can provide separate billing environments, such as development, test, and production. 
	- Policies for individual subscriptions can help satisfy different compliance standards.
	- By using subscriptions, you can manage and track costs for your organizational structure.
    
- **Resource groups** are logical containers into which Azure resources are deployed and managed.
	- Place resources of similar usage, type, or location in logical groups.
	- Organize resources by life cycle so all the resources can be created or deleted at the same time.
	- Apply role permissions to a group of resources or give a group access to administer a group of resources.
	- Use resource locks to protect individual resources from deletion or change.
	- Resources can be moved between resource groups with some exceptions.
	- Resource groups can't be nested.
	- Each resource must be in one, and only one, resource group.
	- Resource groups can't be renamed.
    
- **Resources** are instances of services that you create. For example, virtual machines, storage, and SQL databases.

- Resource Tags
	- **name-value pairs** assigned to Azure resources
	- key tool to organise and manage resources
	- can be applied to resource group, resource, subscription
	- a resource can have up to 50 tags
	- limitation
		- not inherited from resource group to resource
		- tags are case insensitive but stored as entered



## Azure Resource Locks

- prevents accidental deletion or modification of critical Azure resources
- 2 Lock Types:
	- `CanNotDelete` (Delete Lock): authorized users can read and modify the resource normally, but cannot delete it
	- `ReadOnly` (Read-Only Lock): authorized users can only read the resource; blocks all `POST` and `PUT/PATCH/DELETE` ARM control-plane operations

- putting a `ReadOnly` lock 
	- on a Storage Account prevents users from listing storage account keys (`listKeys` is an ARM `POST` operation), which breaks Azure Portal browsing of blobs/containers if using key authentication
	- on a Resource Group containing Virtual Machines prevents starting, stopping, restarting, or resizing VMs
- Resource Locks apply only to the ARM Control Plane 
	- a `CanNotDelete` or `ReadOnly` lock on a SQL Database or Storage Account does not prevent a user with data-plane permissions from running `DROP TABLE` inside SQL or deleting blobs via the data plane
