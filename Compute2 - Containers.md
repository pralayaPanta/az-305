---
tags:
  - studynote
  - az-305
title: Compute - Containers
author: Pralaya Panta
updated: 2026-10-23
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
	- Azure Container Registry (ACR)
		- Basic tier - entry-level for dev/test (10 GB storage)
		- Standard tier - production storage (100 GB) and higher image pull throughput
		- Premium tier - required for Geo-Replication, Private Link (Private Endpoints), Customer-Managed Keys (CMK), and Content Trust / artifact signing (500 GB storage)
		- ACR Tasks - cloud-based container image builds triggered on code commits or base image updates
- image integrity
	- images with vulnerabilities should not be allowed in prod
	- keep number of images small


## Azure Kubernetes Service (AKS)

- orchestration platform
- a portable, extensible open-source platform for automating deployment, scaling, and the management of containerized workloads
- manages hosted Kubernetes env (uses containerd OCI runtime)
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
- node pools
	- system node pool - 
		- hosts critical Kubernetes system pods (CoreDNS, konnectivity, metrics-server); 
		- requires Linux nodes; 
		- use taint CriticalAddonsOnly=true:NoSchedule to block app pods
	- user node pool -
		- hosts application workloads (supports Linux/Windows nodes, Spot node pools, GPU node pools, and Kata Containers pod sandboxing for kernel-level VM isolation per pod)
- network models
	- Kubenet - pods get IPs from internal overlay space and use NAT + UDRs (conserves VNet IPs, legacy)
	- Azure CNI Overlay (recommended default) - nodes get VNet IPs while pods get IPs from a private overlay CIDR (scales to large clusters without exhausting VNet subnet IPs)
	- Azure CNI (flat VNet IP allocation) - every pod receives a routable IP directly from the Azure VNet subnet (requires large subnets)
	- Azure CNI powered by Cilium - combines Azure CNI with eBPF data plane for high-performance routing and network policy enforcement
- can use ARM templates or bicep files to create clusters
- can integrate with Microsoft EntraID
- cluster scaling options
	- horizontal pod auto scaler (HPA) / KEDA
	- cluster auto scaler
	- AKS Virtual Nodes (ACI) - uses Virtual Kubelet to burst stateless pods out to serverless Azure Container Instances in seconds
- manages Kubernetes software upgrades
- supports both static and dynamic storage volumes (Azure Disk CSI for single-pod RWO, Azure Files / NetApp Files CSI for multi-pod RWX)
- integrates with ACR or other repositories



## Azure Container Apps

- fully managed serverless container platform for microservices and background jobs without managing Kubernetes control plane or nodes
- built-in capabilities
	- KEDA (Kubernetes Event-driven Autoscaling) - scales replicas dynamically based on HTTP traffic, CPU/memory, or external event sources (Service Bus queue, Event Hubs, Kafka, Cron) down to zero replicas (scale-to-zero) when idle
	- Dapr (Distributed Application Runtime) - sidecar APIs for service-to-service invocation (mTLS), state management, pub/sub messaging, and secret retrieval
	- Envoy ingress - built-in HTTPS ingress, custom domains, and traffic splitting (Blue/Green and Canary deployments across multiple active revisions)
	- Workload Profiles - supports both Consumption (serverless pay-per-second, scale-to-zero) and Dedicated (single-tenant compute families like D/E/GPU) inside the same environment with VNet integration


## Quick Links

[Azure Container Instances](https://learn.microsoft.com/en-us/azure/container-instances/container-instances-overview)
https://learn.microsoft.com/en-us/training/modules/configure-azure-container-instances/
[security considerations for Azure Container Instances](https://learn.microsoft.com/en-us/azure/container-instances/container-instances-image-security)
[Azure Kubernetes Service](https://learn.microsoft.com/en-us/azure/aks/intro-kubernetes)





