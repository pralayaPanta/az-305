---
tags:
  - studynote
  - az-305
title: Networking - Security
author: Pralaya Panta
updated: 2026-10-23
---


```table-of-contents
```



## Network Security Groups (NSGs)

- filter network traffic to and from Azure resources in an Azure virtual network
- contains a list of access control list (ACL) rules that allow or deny network traffic to subnets, network interface cards (NICs), or both
- NSGs can be associated with either subnets or individual NICs connected to a subnet
- When an NSG is associated with a subnet, the ACL rules apply to all the virtual machines in that subnet.
- evaluation order when applied to both Subnet and NIC (both must allow traffic for it to pass)
	- Inbound traffic: Subnet NSG evaluated first -> NIC NSG evaluated second
	- Outbound traffic: NIC NSG evaluated first -> Subnet NSG evaluated second
- rule sets
	- two sets of rule
		- inbound
		- outbound
	- priority of a rule set must be unique within each set (`100` to `4096`; lower number = higher priority)
	- each rule has 5-tuple (protocol, src IP, src port, dest IP, dest port) + direction, priority, access type (supports Service Tags and Application Security Groups)
	- default rules (`65000+`) cannot be deleted
		- can be assigned lower priority
		- can be override with custom rules


[Azure network security group (NSGs)](https://learn.microsoft.com/en-us/azure/virtual-network/network-security-groups-overview)


## Application Security Groups (ASGs)

- logical grouping of VM network interfaces (NICs) by application role (e.g., `WebServers`, `AppServers`, `DbServers`) within the same Virtual Network
- allows writing NSG rules referencing ASG names (e.g., allow `WebServers` to `DbServers` on TCP `1433`) instead of maintaining hardcoded IP lists when VMs scale out or change IPs


## Azure Firewall

- managed, cloud-based stateful network security service deployed in `AzureFirewallSubnet` (`/26` minimum)
- rule processing order
	- evaluated strictly in order: 1. DNAT rules (inbound port forwarding) -> 2. Network rules (L3/L4 IP, port, protocol) -> 3. Application rules (L7 FQDN/URL/HTTP/S) -> implicit Deny
	- managed centrally across regions/subscriptions using Azure Firewall Policy (supports parent-child policy inheritance)
- tiers
	- basic
		- for SMB environments (up to 250 Mbps; fixed scale unit; alert-only threat intelligence)
	- standard
		- full stateful L3-L7 firewall (up to 30 Gbps)
		- FQDN filtering and FQDN tags
		- log analytics and threat intelligence-based filtering (alert and deny)
	- premium
		- TLS inspection (decrypts outbound/east-west HTTPS traffic using a CA cert in Key Vault)
		- IDPS (signature-based Intrusion Detection and Prevention System)
		- URL filtering (full URL path like `contoso.com/backups` instead of just FQDN) and web categories
		- PCI DSS compliance (scales up to 100 Gbps)

[Introduction to Azure Firewall](https://learn.microsoft.com/en-us/training/modules/introduction-azure-firewall/)

## Service Endpoint and Private Endpoints

- Virtual Network Service Endpoints
	- extends VNet identity to an Azure PaaS service over the Microsoft backbone (adds a system route so traffic leaves from the VNet private IP)
	- PaaS resource still retains its public IP endpoint, does not require Private DNS, cannot be reached directly from on-premises over VPN/ExpressRoute (unless using Service Endpoint Policies / NAT), and has zero additional cost
- Azure Private Endpoints (Azure Private Link)
	- creates a NIC with a private IP address inside your VNet subnet mapped to a specific PaaS resource instance (Storage, SQL, Key Vault, Cosmos DB)
	- allows completely disabling public network access on the PaaS resource (`Public network access: Disabled`), is fully routable from peered VNets and on-premises over VPN/ExpressRoute, blocks data exfiltration to unauthorized accounts, and requires Private DNS (`privatelink.*`)


## Azure DDoS Protection

- tiers
	- DDoS Network Protection
		- VNet-level protection plan covering all public IPs in enabled VNets; includes adaptive real-time tuning, WAF discount, DDoS Rapid Response (DRR) support, and cost protection credits
	- DDoS IP Protection
		- Pay-per-protected-Public-IP model with the same core DDoS mitigation engine, no VNet plan fee, suitable for a few individual Public IPs (excludes DRR support and cost protection)

## Web Application Firewall (WAF)

- provides Layer-7 protection to web apps from common web exploits and vulnerabilities (SQL injection, XSS, OWASP Top 10, rate limiting, geo-filtering, bot protection)
- deployment locations
	- WAF on Azure Front Door - global edge protection before traffic enters your VNet/region
	- WAF on Azure Application Gateway - regional VNet-level protection for internal or regional web workloads

[Azure Web Application Firewall (WAF)](https://learn.microsoft.com/en-us/azure/web-application-firewall/overview)


## Azure Private Link

- access Azure PaaS services and Azure hosted customer-owned/partner services over a private endpoint
- traffic between  virtual network and the service travels the Microsoft backbone network
	- restrict traffic to the Microsoft network with no public internet access.


[Azure Private Link](https://learn.microsoft.com/en-us/azure/private-link/private-link-overview)