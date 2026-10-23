---
tags:
  - studynote
  - az-305
title: Networking - Architecture Best Practices
author: Pralaya Panta
updated: 2026-10-23
---


```table-of-contents
```


## Requirements to know

- Naming convention
- Azure region for resources
- Subscriptions
- IP address requirement
- Segmentation
- Network security and traffic filtering


## Plan IP addressing for virtual networks

- Limit Virtual Network address spaces to a maximum CIDR range of `/16` (65,536 IP addresses) to avoid wasting private IP addresses
	- Do not assign prefix ranges larger than `/16` (such as `/15`), as the excess IP addresses become unusable elsewhere in your organization
- Ensure Virtual Network address ranges never overlap with on-premises networks to avoid broken routing and connectivity failures
	- Redesign the network if IP ranges overlap, relying on Network Address Translation (NAT) strictly as a last resort


## Hub-spoke network

- hub and spoke network topology isolates workloads while sharing services, such as identity and security
- hub acts as central point of connectivity
- spokes are virtual networks that connect to the hub virtual network by using peering


## Network Patterns


### Single Vnet
- all components are placed in a single virtual network
- use of NSG and ASG for security
- examples
	- 1 subnet can contain db
	- another subnet can contain web
	- enforce segmentation using NVA or firewall
	- govern traffic using NSG

### Multiple Vnets with peering in between them
- built-in segmentation through virtual networks
- can add more segmentations within a virtual network in a manner similar to pattern 1 by using NSGs
- can use peering to make connection between Vnets if required


### Multiple Vnets in a hub and spoke model
- a virtual network in a given region as the hub for all the other virtual networks in that region
- connectivity between the hub virtual network and its spoke virtual networks is achieved by using Virtual Network peering
- traffic passes through the hub virtual network, and it can act as a gateway to other hubs in different regions
- a layer 3 router (such as Azure Firewall) in the hub virtual network is required to enable connectivity


## Azure Virtual Network Manager (AVNM)

- centralized network management service used to group, configure, deploy, and manage Virtual Networks at scale across subscriptions and regions
- Network Groups:
	- logical containers of Virtual Networks with static membership (manually added) or dynamic membership (automatically added via Azure Policy based on tags, regions, or subscriptions)
- Connectivity Configurations:
	- automates creating and maintaining Hub-and-Spoke peering topologies (including optional Direct Connectivity between spokes in the same group without traversing the hub) or Full Mesh topologies at scale
- Security Admin Rules:
	- centrally enforced baseline security rules applied across Network Groups that are evaluated before Network Security Groups (NSGs)
	- rule actions:
		- `Deny`: blocks traffic immediately; spoke VNet owners cannot override this with an NSG
		- `Always Allow`: forces traffic to be permitted; bypasses NSG evaluation completely so spoke owners cannot accidentally block critical central IT traffic (such as monitoring, patching, or DNS)
		- `Allow`: permits traffic at the AVNM layer and hands it down to the spoke NSG for standard evaluation
- Routing Configurations:
	- centrally manages User-Defined Routes (UDRs) across Network Groups to enforce consistent next-hop routing through a central Azure Firewall or NVA



