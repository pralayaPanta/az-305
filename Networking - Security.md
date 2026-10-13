---
tags:
  - studynote
  - az-305
title:
author: Pralaya Panta
updated: 2026-09-27
---


```table-of-contents
```



## Network Security Groups (NSGs)

- filter network traffic to and from Azure resources in an Azure virtual network
- contains a list of access control list (ACL) rules that allow or deny network traffic to subnets, network interface cards (NICs), or both
- NSGs can be associated with either subnets or individual NICs connected to a subnet
- When an NSG is associated with a subnet, the ACL rules apply to all the virtual machines in that subnet.
- rule sets
	- two sets of rule
		- inbound
		- outbound
	- priority of a rule set must be unique within each set
	- each rule has protocol, src, dest port, address prefix, direction, priority, access type
	- default rules cannot be deleted
		- can be assigned lower priority
		- can be override with custom rules


[Azure network security group (NSGs)](https://learn.microsoft.com/en-us/azure/virtual-network/network-security-groups-overview)


## Application Security Groups (ASGs)



## Azure Firewall

- managed, cloud-based network security service
- tiers
	- basic
		- limited feature alert only
		- not for prod
	- standard
		- full stateful firewall
		- FQDN filtering
		- log analytics and threat intelligence
	- premium
		- TLS inspection with IDPS
		- URL Filtering
		- PCI DSS compliance

[Introduction to Azure Firewall](https://learn.microsoft.com/en-us/training/modules/introduction-azure-firewall/)

## Service Endpoint and Private Endpoints


## Azure DDoS Protection

- tiers
	- **DDoS Network Protection**: 
		- VNet-level protection plan covering multiple resources, includes DDoS Rapid Response support and cost protection guarantees
	- **DDoS IP Protection**: 
		- Pay-per-protected-IP model, no protection plan required, suitable for individual workloads.

## Web Application Firewall (WAF)

- provides protection to web app from common web exploits and vulnerabilities
- like
	- SQL injection
	- Cross site scripting
-  out of box protection from OWASP top 10 vulnerabilities via managed rules

[Azure Web Application Firewall (WAF)](https://learn.microsoft.com/en-us/azure/web-application-firewall/overview)


## Azure Private Link

- access Azure PaaS services and Azure hosted customer-owned/partner services over a private endpoint
- traffic between  virtual network and the service travels the Microsoft backbone network
	- restrict traffic to the Microsoft network with no public internet access.


[Azure Private Link](https://learn.microsoft.com/en-us/azure/private-link/private-link-overview)