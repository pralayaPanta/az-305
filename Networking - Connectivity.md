---
tags:
  - studynote
  - az-305
title: Networking - Connectivity
author: Pralaya Panta
updated: 2026-10-23
---

```table-of-contents
```




## Azure ExpressRoute

- uses a private, dedicated Layer-3 connection through a connectivity provider 
- 50 Mbps to 10 Gbps; 
- redundant active-active circuits
- traffic does not go over internet - private
- private connection extends on-prem network into Azure
- peering domains
	- Azure Private Peering - connects to private IPs inside Azure VNets
	- Microsoft Peering - connects to public endpoints of PaaS/SaaS services (Microsoft 365, Azure public services)
- SKUs & advanced features
	- Local SKU (unlimited data to 1-2 local regions), Standard SKU (geo-political region), Premium Add-on (global VNet access across geographies, >4,000 BGP routes)
	- ExpressRoute Global Reach - links on-premises sites together directly across the Microsoft global backbone between ExpressRoute circuits
	- ExpressRoute FastPath - bypasses the ExpressRoute Virtual Network Gateway for the data plane, sending traffic directly to VMs in the VNet for highest throughput and lowest latency (requires UltraPerformance or ErGw3AZ gateway)
	- ExpressRoute Direct - dual 10 Gbps or 100 Gbps physical ports connected directly into Microsoft's global network routers

[Hybrid network with Azure ExpressRoute](https://learn.microsoft.com/en-us/azure/architecture/reference-architectures/hybrid-networking/expressroute)
## Azure VPN Gateway

- sends encrypted IPsec/IKE traffic between an Azure virtual network and an on-premises location over the public internet (deployed in `GatewaySubnet`)
- connection type
	- site to site (S2S - IPsec/IKEv2 tunnel to on-prem VPN device)
	- point to site (P2S - remote client via OpenVPN, SSTP, or IKEv2 with Entra ID / certificate / RADIUS auth)
	- virtual network to network (VNet-to-VNet)
- VPN types & high availability
	- Route-based VPN (recommended) - uses dynamic routing/any-to-any wildcard traffic selectors (IKEv2); required for BGP, ExpressRoute coexistence, P2S, and Zone-Redundant SKUs (VpnGw1AZ-VpnGw5AZ)
	- Policy-based VPN (legacy) - static traffic selectors (IKEv1); max 1 S2S tunnel, no BGP
	- Active-Active mode - both gateway instances establish active S2S tunnels simultaneously (combine with dual on-prem VPN devices + BGP for full redundancy)

[Hybrid network with Azure VPN Gateway](https://learn.microsoft.com/en-us/azure/architecture/reference-architectures/hybrid-networking/#vpn-connection)


## Azure ExpressRoute with VPN failover
- Combine Azure ExpressRoute and Azure VPN Gateway in the same `GatewaySubnet` to automatically fail over via BGP to an IPsec S2S VPN connection over the internet if the ExpressRoute circuit goes down

[Hybrid network with Azure ExpressRoute and VPN failover](https://learn.microsoft.com/en-us/azure/architecture/reference-architectures/hybrid-networking/expressroute-vpn-failover)

## Virtual WAN

- networking service that provides optimized and automated hub-and-spoke branch connectivity to, and through, Azure
- Azure Virtual WAN brings together many Azure cloud connectivity services such as site-to-site VPN, ExpressRoute, point-to-site user VPN into a single operational interface
- Connectivity to Azure VNets is established by using virtual network connections.
- tiers
	- Basic Virtual WAN - Site-to-Site VPN only in a single hub
	- Standard Virtual WAN - supports ExpressRoute, P2S User VPN, S2S VPN, full-mesh any-to-any transitive routing across VNet spokes and branches, Multi-Hub global transit, and Routing Intent / Secured Virtual Hub (integrated Azure Firewall)

[Hub-spoke topology with Azure Virtual WAN](https://learn.microsoft.com/en-us/azure/architecture/networking/hub-spoke-vwan-architecture)
## Network Routing

- why modify network
	- network topology
	- traffic isolation and segmentation
	- forced tunneling
	- integrate with on-prem network
- route order
	- Azure selects next hop based on the longest prefix match (e.g., `/24` beats `/16`)
	- if there are multiple hop entries with same prefix
		- UDRs, then BGP, then system routes

### System Routes
- default routes created by azure while creating VNets
- defined for a specific location when they're created
- can't be modified after they're created, 
	- can override these routes by configuring UDRs

### User Defined Routes (UDR)
- custom routing rules defined by users in a Route Table associated with subnets
- create UDRs in Azure to override Azure's default system routes (e.g., `0.0.0.0/0 -> VirtualAppliance` pointing to Azure Firewall private IP for forced inspection)
- Propagate gateway routes toggle - set to `No` on spoke subnets if you want to prevent on-premises BGP routes from bypassing the hub firewall


### Border Gateway Protocol (BGP)
- dynamic routing protocol that automatically exchanges IP prefixes between on-premises routers and Azure VPN Gateways / ExpressRoute Gateways (Azure default ASN is `65515`)
- eliminates manual route table updates when new subnets are added and enables automatic failover between active-active tunnels or ExpressRoute + VPN backup
- VNet Peering gotcha - VNet peering is non-transitive by default; to allow spokes to use the hub's VPN/ExpressRoute gateway, enable `Allow gateway transit` on the hub peering and `Use remote gateways` on the spoke peering


## Hybrid DNS & Performance Optimization
- Azure DNS Private Resolver - managed PaaS DNS proxy/forwarder inside a VNet (Inbound Endpoint for on-prem queries to Azure Private DNS Zones; Outbound Endpoint + Forwarding Rulesets for Azure queries to on-prem AD DNS without running custom DNS VMs)
- Accelerated Networking (SR-IOV) - enables Single Root I/O Virtualization on supported VM NICs so network packets bypass the hypervisor host virtual switch and go directly to physical NIC hardware for lower latency, reduced jitter, and lower CPU usage

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

- SNAT port exhaustion problem - occurs when VMs or App Service/AKS instances make many concurrent outbound calls to the same destination IP:Port and run out of pre-allocated SNAT ports
- Azure NAT Gateway (recommended) - managed outbound-only PaaS service attached at the subnet level; provides 64,512 SNAT ports per Public IP (supports up to 16 Public IPs / Public IP Prefix for >1M dynamic SNAT ports shared on-demand across the subnet)
- Standard Load Balancer outbound rules - explicit SNAT configuration for backend pool VMs
- Azure Firewall / Network Virtual Appliances (NVAs) - centralized outbound egress filtering and SNAT in hub-spoke topologies
- Note - default implicit outbound internet access for new VMs without an explicit outbound method is deprecated in Azure

