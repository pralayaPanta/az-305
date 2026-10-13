---
tags:
  - studynote
  - az-305
title:
author: Pralaya Panta
updated: 2026-10-01
---


```table-of-contents
```



## Azure Container Instances

- fast and simple way to run container on Azure
- implements per second billing
	- costs only when container is running
- supports custom size
	- can specific exact value for CPU cores and memory
- persistent storage
- file share can be mounted directly to container
- can be used with Linux and Windows
- container group
	- collection of containers that get scheduled on the same host machine
	- share a lifecycle, resources, local network, and storage volumes
	- multi container group
		- useful to divide a single functional task into several image
		- ex - a container serving a web application and a container pulling the latest content from source control
- private registry
	- Docker Trusted Registry
	- Azure Container Registry
- image integrity
	- images with vulnerabilities should not be allowed in prod
	- keep number of images small


## Azure Kubernetes Instances

- orchestration platform
- a portable, extensible open-source platform for automating deployment, scaling, and the management of containerized workloads
- manages hosted Kubernetes env
- features
	- automated update
	- self healing
	- easy scaling
- tier
	- free
		- no control plane charge with no SLA
		- suitable for dev./test
	- standard
		- hourly control plane charge with uptime SLA
		- recommended for prod
	- premium
		- adds 24 months long term support
- can use ARM templates or bicep files to create clusters
- can integrate with Microsoft EntraID
- cluster scaling options
	- horizontal pod auto scaler
	- cluster auto scaler
- manages Kubernetes software upgrades
- supports both static and dynamic storage volumes
- supports the Docker file image format by default
- integrates with ACR or other repositories



## Azure Container Apps




## Quick Links

[Azure Container Instances](https://learn.microsoft.com/en-us/azure/container-instances/container-instances-overview)
https://learn.microsoft.com/en-us/training/modules/configure-azure-container-instances/
[security considerations for Azure Container Instances](https://learn.microsoft.com/en-us/azure/container-instances/container-instances-image-security)
[Azure Kubernetes Service](https://learn.microsoft.com/en-us/azure/aks/intro-kubernetes)



![[Pasted image 20261004091820.png]]


![[Pasted image 20261004092923.png]]