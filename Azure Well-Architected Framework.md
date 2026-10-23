---
tags:
  - studynote
  - az-305
title: Azure Well-Architected Framework
author: Pralaya Panta
updated: 2026-10-23
---

```table-of-contents
```


## Quick Links
[Azure Well-Architected Framework](https://learn.microsoft.com/en-us/azure/well-architected/)
[Build great solutions with the Microsoft Azure Well-Architected Framework](https://learn.microsoft.com/en-us/training/paths/azure-well-architected-framework/)

## Pillars Overview

- Reliability
	- designed to anticipate failure at all levels and self heal
- Security
	- resilient to security concerns
	- building with a Zero Trust approach and following the CIA Triad: confidentiality, integrity, and availability.
- Performance efficiency
	- workload can handle changes in demand
		- if there's an increase in load, your workload needs to manage it without affecting user experience.
		- when the load decreases, it's important to conserve resources.
- Operational excellence
	- keep everything on track to ensure high-quality performance
- Cost optimisation
	- making sure the money that your organization spends is put to good use
	- architecture design is driven by business goals and must factor in return on investment (ROI) and financial constraints


## Reliability
- design for business requirements (define realistic SLA, RTO, and RPO targets per workload tier)
- design for redundancy and fault tolerance across Availability Zones and paired regions (avoid single points of failure)
- implement self-healing patterns: retry with exponential backoff, circuit breaker, queue-based load leveling, and health endpoint monitoring
- test resiliency continuously using Azure Chaos Studio and regular disaster recovery drills (ASR Test Failover)

## Security
- adopt Zero Trust principles: verify explicitly, enforce least privilege (JIT via PIM, custom RBAC/ABAC), and assume breach
- eliminate secrets in code by using Managed Identities, Workload Identity Federation, and Azure Key Vault
- isolate network boundaries using Private Link / Private Endpoints, NSGs/ASGs, Azure Firewall, and WAF (Front Door / Application Gateway)
- monitor posture and threats continuously with Microsoft Defender for Cloud (Secure Score, CWPP) and Microsoft Sentinel

## Performance efficiency
- scale horizontally (scale out/in via VMSS, AKS/KEDA, App Service autoscale) rather than only scaling vertically
- optimize data and network latency using caching (Azure Managed Redis Cache-Aside, Front Door CDN), read replicas, and partitioning/sharding
- offload heavy background processing asynchronously using queues (Service Bus / Storage Queues) and serverless workers

## Operational excellence
- standardize deployments using Infrastructure as Code (Bicep, ARM Templates, Terraform, Deployment Stacks) and automated CI/CD pipelines
- implement end-to-end observability across logs, metrics, and distributed traces (Azure Monitor, Log Analytics, Application Insights)
- use safe deployment practices: Blue/Green slot swaps, Canary traffic splitting, and Feature Flags (Azure App Configuration)

## Cost optimisation
- right-size underutilized compute and databases using Azure Advisor and performance-based sizing
- use commitment discounts: 1-year or 3-year Azure Reservations and Azure Savings Plans for Compute, plus Azure Hybrid Benefit (BYOL)
- match storage and compute tiers to usage patterns: Serverless/Consumption for spiky workloads, Spot VMs for interruptible batch jobs, and Blob Lifecycle Management (Hot -> Cool -> Cold -> Archive)
- enforce financial governance using resource tags, Azure Cost Management budgets, and cost anomaly alerts

