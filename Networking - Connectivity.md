---
tags:
  - studynote
  - az-305
title:
author: Pralaya Panta
---

```table-of-contents
```




## Azure ExpressRoute

- uses a private, dedicated connection through a non-Microsoft connectivity provider
- traffic does not go over internet - private
- private connection extends on-prem network into Azure

[Hybrid network with Azure ExpressRoute](https://learn.microsoft.com/en-us/azure/architecture/reference-architectures/hybrid-networking/expressroute)
## Azure VPN Gateway

- sends encrypted traffic between an Azure virtual network and an on-premises location
- encrypted traffic goes over the public internet
- connection type
	- site to site
	- point to site
	- virtual network to network

[Hybrid network with Azure VPN Gateway](https://learn.microsoft.com/en-us/azure/architecture/reference-architectures/hybrid-networking/#vpn-connection)


## Azure ExpressRoute with VPN failover
- Combine Azure ExpressRoute and Azure VPN Gateway to create a failover to a VPN connection if there's a loss of connectivity in the ExpressRoute circuit

[Hybrid network with Azure ExpressRoute and VPN failover](https://learn.microsoft.com/en-us/azure/architecture/reference-architectures/hybrid-networking/expressroute-vpn-failover)

## Virtual WAN

- networking service that provides optimized and automated branch connectivity to, and through, Azure
- Azure Virtual WAN brings together many Azure cloud connectivity services such as site-to-site VPN, ExpressRoute, point-to-site user VPN into a single operational interface
- Connectivity to Azure VNets is established by using virtual network connections.

[Hub-spoke topology with Azure Virtual WAN](https://learn.microsoft.com/en-us/azure/architecture/networking/hub-spoke-vwan-architecture)
## Network Routing

- why modify network
	- network topology
	- traffic isolation and segmentation
	- forced tunneling
	- integrate with on-prem network
- route order
	- Azure selects next hop based on the longest prefix match 
	- if there are multiple hop entries with same prefix
		- UDRs, then BGP, then system routes

### System Routes
- default routes created by azure while creating VNets
- defined for a specific location when they're created
- can't be modified after they're created, 
	- can override these routes by configuring UDRs

### User Defined Routes (UDR)
- custom routing rules defined by users - *custom routes*
- create UDRs in Azure to override Azure's default system routes


### Border Gateway Protocol (BGP)



## Azure Private Link

- access Azure Services over a private endpoint in virtual network
- traffic travels Microsoft backbone network
- private endpoint is mapped to an instance of a PaaS resource instead of entire service


- private endpoint
	- network interface that uses private IP address to connect privately to a service
	- can be storage, Cosmos DB etc
	- enables connectivity between vNets, peers vNets, onprem environments
	- subs with private link resource must be registered with MS network resource provider

## Outbound connectivity

- load balancer
- azure NAT gateway
- azure firewall
- network virtual appliances (NVAs)

