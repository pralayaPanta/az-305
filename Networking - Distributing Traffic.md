---
tags:
  - studynote
  - az-305
title: Networking - Distributing Traffic
author: Pralaya Panta
updated: 2026-10-23
---

```table-of-contents
```


# Distribute Regional Traffic


## Azure Load Balancer

- Regional (and Cross-Region Global SKU)
- Layer 4 (TCP/UDP)
- Internal and external load balancing for non-HTTP(S) traffic (ultra-low latency)
- High availability, low latency, zonal and zone-redundant endpoints (`99.99% SLA` on Standard SKU; Basic SKU is retired)
- SKUs & types
	- Standard Load Balancer - secure by default (requires NSG allow rules), supports up to 1,000 backend VMs/VMSS in a single VNet, HA Ports (`0` all ports for NVAs), and Outbound Rules
	- Gateway Load Balancer - transparently chains third-party Layer-3/Layer-4 Network Virtual Appliances (firewalls, packet inspection appliances) into the traffic path via VXLAN encapsulation
	- Cross-Region Load Balancer - global Layer-4 frontend that distributes traffic across regional Standard Load Balancers with client IP preservation
- cost is based on rules and data processed

[Introduction to Azure Load Balancer](https://learn.microsoft.com/en-us/training/modules/intro-to-azure-load-balancer/)
## Application Gateway

- Regional
- Layer 7 (HTTP/HTTPS/WebSockets/HTTP2)
- Web application firewall, TLS/SSL termination (or end-to-end TLS re-encryption), and HTTP load balancing
- Path-based routing (`/images/*` vs `/video/*`), multi-site host-header routing, URL/header rewrite, cookie-based session affinity, and HTTP-to-HTTPS redirection
- Standard_v2 and WAF_v2 SKUs support zone redundancy and autoscaling (requires dedicated subnet, `/24` recommended)

[Introduction to Azure Application Gateway](https://learn.microsoft.com/en-us/training/modules/intro-to-azure-application-gateway/)


# Distribute Global Traffic



## Azure Front Door

- Global CDN/ADN
- Layer 7 (HTTP/HTTPS)
- Uses global Anycast network and Split TCP to terminate client TLS at the nearest Microsoft edge POP and route over the Microsoft global backbone
- Path-based routing, TLS/SSL offload, Web Application Firewall (WAF), URL-based routing, CDN/edge caching, and fast health-probe failover
- tier
	- standard - content delivery (CDN) optimization, custom domains, free managed TLS certs, basic custom WAF rules
	- premium - adds OWASP managed WAF rulesets, Bot Manager protection, and Azure Private Link integration to connect directly to internal origins (private App Services, Internal Load Balancers, Storage Accounts, or APIM) without exposing public origin IPs

## Azure Traffic Manager

- Global
- DNS-based load balancer (operates at the DNS resolution layer for any TCP/UDP protocol; does not proxy traffic — clients connect directly to the resolved endpoint)
- 6 routing methods
	- Priority - active-passive failover to backup endpoint if primary fails health checks
	- Weighted - distributes traffic by percentage (e.g., 90/10 blue-green or canary testing)
	- Performance - routes users to the endpoint with the lowest network latency
	- Geographic - routes users by geographic location (for data sovereignty/localization)
	- MultiValue - returns multiple healthy IPv4/IPv6 addresses in a single DNS response
	- Subnet - maps specific client source IP ranges to designated endpoints


## Azure Content Delivery Network (CDN) & Load Balancing Selection

- Azure Front Door Standard / Premium is the current unified global CDN and Layer-7 load balancer (legacy Azure CDN from Edgio and Microsoft Classic CDN are retired)
- caches static assets (images, video, CSS, JS) at Microsoft global edge POPs to reduce origin load and latency
- 4-way load balancer decision summary
	- Global + HTTP/HTTPS (Layer 7 + WAF + CDN + Split TCP) -> Azure Front Door
	- Global + Non-HTTP / Any protocol (DNS-level failover/routing) -> Azure Traffic Manager (or Cross-Region Load Balancer for L4 IP preservation)
	- Regional + HTTP/HTTPS (Layer 7 + WAF + URL path/host routing) -> Azure Application Gateway
	- Regional + Non-HTTP TCP/UDP (Layer 4 ultra-low latency) -> Azure Standard Load Balancer





