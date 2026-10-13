---
tags:
  - studynote
  - az-305
title:
author: Pralaya Panta
updated: 2026-09-13
---

```table-of-contents
```


# Distribute Regional Traffic


## Azure Load Balancer

- Regional/Global
- Layer 4 (TCP/UDP)
- Internal and external load balancing for non-HTTP(S) traffic
- High availability, low latency, zonal and zone-redundant endpoints
- cost is based on rules and data processed

[Introduction to Azure Load Balancer](https://learn.microsoft.com/en-us/training/modules/intro-to-azure-load-balancer/)
## Application Gateway

- Regional
- Layer 7 (HTTP/HTTPS)
- Web application firewall, TLS/SSL termination, and HTTP load balancing
- Path-based routing, TLS/SSL offload, Web Application Firewall (WAF), URL-based routing
- 

[Introduction to Azure Application Gateway](https://learn.microsoft.com/en-us/training/modules/intro-to-azure-application-gateway/)


# Distribute Global Traffic



## Azure Front Door

- Global CDN/ADN
- Layer 7 (HTTP/HTTPS)
- Path-based routing, TLS/SSL offload, Web Application Firewall (WAF), URL-based routing, CDN/edge caching
- tier
	- standard
	- premium

## Azure Traffic Manager

- Global
- DNS-based
	- DNS-based routing, geographic routing, priority routing, weighted routing
- 


## Azure Content Delivery Network (CDN)





![[Pasted image 20261004134312.png]]