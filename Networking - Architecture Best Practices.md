---
tags:
  - studynote
  - az-305
title:
author: Pralaya Panta
updated: 2026-10-04
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


* Limit Virtual Network address spaces to a maximum CIDR range of /16 (65,536 IP addresses) to avoid wasting private IP addresses.
	* Do not assign prefix ranges larger than /16 (such as /15), as the excess IP addresses become unusable elsewhere in your organization.
* Ensure Virtual Network address ranges never overlap with on-premises networks to avoid broken routing and connectivity failures.
	* Redesign the network if IP ranges overlap, relying on Network Address Translation (NAT) strictly as a last resort.


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
- a layer 3 router (such as Azure Firewall) in the hub virtual network is required to enable connectivity.



