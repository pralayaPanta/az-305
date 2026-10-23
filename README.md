---
tags:
  - mscert
  - az-305
  - azure
  - architecture
  - study-guide
created: 2026-08-18
type: cert-guide
---

# ☁️ AZ-305: Designing Microsoft Azure Infrastructure Solutions

> Complete study guide, domain breakdown, architectural notes, and 494-question practice examination suite for the **Microsoft Certified: Azure Solutions Architect Expert** certification.

---

## 📌 Certification Overview

- **Target Credential:** [Microsoft Certified: Azure Solutions Architect Expert](https://learn.microsoft.com/en-us/credentials/certifications/azure-solutions-architect/)
- **Exam Code:** AZ-305
- **Exam Result:** ✅ **Passed — 811 / 1000** (Oct 16, 2026 | Valid through Oct 16, 2027)
- **Prerequisites:** [AZ-104: Microsoft Azure Administrator](https://learn.microsoft.com/en-us/credentials/certifications/azure-administrator/)
- **Passing Score:** 700 / 1000
- **Question Format:** 40–60 questions (Case studies, multiple-choice, multi-select, drag-and-drop, hot-area)
- **Exam Time:** 100–120 minutes
- **Curriculum Baseline:** Updated for 2026 objectives (incorporating Azure AI Foundry, Copilot Studio, Virtual WAN routing intents, and FinOps)

---

## ⚖️ Exam Domain Weights

| Domain | Weight | Key Concepts |
|---|:---:|---|
| **1. Design Identity, Governance & Monitoring** | **25–30%** | Entra ID, Single vs Multi-Tenant, Conditional Access, Managed Identities, Azure Policy, CAF Landing Zones, Azure Monitor Agent & Log Analytics |
| **2. Design Data Storage Solutions** | **20–25%** | Cosmos DB (5 consistency levels, partition keys), Azure SQL (Hyperscale, Business Critical, Managed Instance), Blob Storage tiers & WORM, ADLS Gen2 |
| **3. Design Business Continuity Solutions** | **15–20%** | Azure Site Recovery (ASR), Azure Backup (immutable vaults, CRR, MUA), Auto-Failover Groups, multi-region resilience & RTO/RPO |
| **4. Design Infrastructure Solutions** | **30–35%** | Compute decision tree (VMs, VMSS, AKS, Container Apps, ACI, Functions), Global/Regional load balancers (Front Door, App Gateway), Virtual WAN, ExpressRoute |

---

## 📚 Technical Study Notes Directory

Comprehensive architectural reference notes organized strictly by official AZ-305 exam domains:

### 🏛️ Foundations & Architecture Strategy
- [Architectural Foundations & High Availability](A%20-%20Introduction%20and%20Basic%20Concepts.md) — High availability principles, Availability Zones, Availability Sets, and foundational design concepts.
- [Azure Well-Architected Framework](Azure%20Well-Architected%20Framework.md) — The 5 WAF pillars: Reliability, Security, Cost Optimization, Operational Excellence, and Performance Efficiency.
- [Master Study Plan](305-study-plan.md) — Detailed week-by-week syllabus, topic schedules, and architectural trade-off review guide.
- [Curated Study Links](Study%20Links.md) — Microsoft Architecture Center decision trees, official exam sandboxes, and reference guides.
- [AZ-305 NotebookLM Study Notebook](https://notebook.google.com/notebook/2c777205-dcdd-4cce-a869-8e3f84efb353?utm_source=gemini_notebook&utm_medium=referral&pli=1) — Interactive AI notebook and audio review source for AZ-305 architectural concepts.

### Domain 1: Design Identity, Governance & Monitoring (25–30%)
- [Identity & Authentication](Identity%20-%20Authentication.md) — Microsoft Entra ID architectures, B2B/B2C/External ID, Managed Identities, and OAuth 2.0 / OIDC delegated flows.
- [Conditional Access & Authentication Strengths](Identity%20-%20Conditional%20Access.md) — Signals, P1/P2/Workload ID licensing, Grant/Session controls, Continuous Access Evaluation (CAE), and Phishing-Resistant MFA.
- [Microsoft Entra Domain Services](Identity%20-%20Entra%20Domain%20Services.md) — Managed domain services (LDAP/LDAPS, Kerberos, NTLM), SKUs, one-way sync, and Replica Sets.
- [Azure Key Vault & Managed HSM](Identity%20-%20Azure%20Key%20Vault.md) — Secrets, keys, certificates, Standard vs. Premium vs. Managed HSM, Soft Delete, Purge Protection, and Key Vault RBAC roles.
- [Microsoft Entra ID Governance](Identity%20-%20Governance.md) — Entitlement Management (Access Packages), Access Reviews, Privileged Identity Management (PIM), and Lifecycle Workflows.
- [Governance & Azure Policy](Governance%20-%20Azure%20Policy.md) — Management Group hierarchies, Azure Policy initiatives, subscription vending, and tag governance.
- [Monitoring Solutions](Monitoring%20-%20Solutions.md) — Azure Monitor Agent (AMA), Data Collection Rules, centralized Log Analytics, and KQL schemas.

### Domain 2: Design Data Storage Solutions (20–25%)
- [Storage Architecture Introduction](Storage1%20-%20introduction.md) — Storage account types, redundancy models (LRS, ZRS, GRS, GZRS, RA-GRS), and network isolation.
- [Non-Relational Blob Storage](Storage2%20-%20non%20relational%20data.md) — Hot, Cool, Cold, Archive tiers, high-priority rehydration, and WORM immutability.
- [Relational Databases & Azure SQL](Storage3%20-%20relational%20database.md) — Azure SQL Database, Hyperscale, Business Critical, and Azure SQL Managed Instance.
- [NoSQL & Azure Cosmos DB](Storage4%20-%20non%20relational%20database%20(nosql).md) — Cosmos DB NoSQL API, 5 consistency levels, partition key design, multi-region writes, and Integrated Cache.
- [Data Integration & Analytics](Storage5%20-%20data%20integration.md) — Synapse Link, Event Hubs Capture, ADLS Gen2 POSIX ACLs, and Azure Databricks Unity Catalog.

### Domain 3: Design Business Continuity Solutions (15–20%)
- [Backup & Disaster Recovery](Business%20Cont%20-%20Backup%20&%20recovery.md) — Azure Backup, Recovery Services Vaults vs. Backup Vaults, Cross-Region Restore (CRR), Multi-User Authorization (MUA), ASR, and SQL Auto-Failover Groups.

### Domain 4: Design Infrastructure Solutions (30–35%)

#### Compute, Serverless & Application Architecture
- [Virtual Machines Architecture & Sizing](Compute1%20-%20Virtual%20Machines.md) — VM family decision tree, Managed Disks, Host Caching gotchas, SLAs (Single VM, AvSets, AvZones), VMSS Flexible vs Uniform, and Host Encryption.
- [Containers & Microservices](Compute2%20-%20Containers.md) — Container decision matrix, Azure Container Instances (ACI), Azure Kubernetes Service (AKS), and Azure Container Apps (ACA).
- [Serverless & Functions](Compute3%20-%20Serverless.md) — Azure App Service, Azure Functions (Consumption, Flex Consumption, Elastic Premium, Dedicated, Durable Functions), and Logic Apps.
- [Application Architecture & Services](App%20Architecture%20and%20services.md) — Messaging (Service Bus vs. Storage Queues), Eventing (Event Grid vs. Event Hubs), API Management (APIM), Azure Cache for Redis, and Azure App Configuration.
- [App & Infrastructure as Code (IaC)](App%20&%20Infrastructure%20as%20Code%20(IaC).md) — ARM Templates, Bicep, Template Specs, Azure Deployment Stacks, Azure Automation, Azure DevOps, and GitHub Actions.

#### Migrations
- [Migration Design & Tools](Migration%20-%20Design%20and%20Tools.md) — Cloud Adoption Framework (CAF), Azure Migrate, Database Migration Service (DMS), Storage Mover, File Sync, Import/Export, and Azure Data Box.

#### Networking & Global Traffic
- [Networking Architecture Best Practices](Networking%20-%20Architecture%20Best%20Practices.md) — IP addressing (`/16` max, non-overlapping CIDR), Single-VNet, Multi-VNet Peering, and Hub-Spoke topologies.
- [Distributing Regional & Global Traffic](Networking%20-%20Distributing%20Traffic.md) — Azure Load Balancer (Standard/Gateway), Application Gateway (WAF v2), Azure Front Door, Traffic Manager, and Azure CDN.
- [Hybrid Connectivity & Routing](Networking%20-%20Connectivity.md) — ExpressRoute, VPN Gateway, Virtual WAN, BGP routing, User Defined Routes (UDRs), and Azure NAT Gateway.
- [Network Security & Isolation](Networking%20-%20Security.md) — Network Security Groups (NSGs), Application Security Groups (ASGs), Azure Firewall, Private Endpoints, Service Endpoints, and DDoS Protection.

---

## 🎯 494-Question Interactive Practice Exam Suite

Nine full-length 50-question mock exams plus two 22-question enterprise case studies testing senior architectural decision-making. All questions feature interactive expandable answer toggles for active-recall self-testing.

- **[AZ-305 Quiz & Benchmark Tracker](quiz/az-305-quiz.md)** — Score log, test-taking strategy, and question bank navigation.

| Mock Exam | Question Count | Core Focus & Tested Topics |
|---|:---:|---|
| **[Practice Set 1](quiz/az-305-practice-set-1.md)** | **50 Questions** | Core Architectural Foundations: Identity, Governance, Storage Tiers, Compute, BCDR & Edge Routing. |
| **[Practice Set 2](quiz/az-305-practice-set-2.md)** | **50 Questions** | Management Group Scopes, Activity Logs, APIM, Managed Identities, Key Vault RBAC & Storage Encryption. |
| **[Practice Set 3](quiz/az-305-practice-set-3.md)** | **50 Questions** | Dynamic Data Masking, Entitlement Management, Hybrid Storage, Azure AI Search (Hybrid/Semantic) & Bastion. |
| **[Practice Set 4](quiz/az-305-practice-set-4.md)** | **50 Questions** | **2026 Objectives:** Azure AI Foundry & Copilot Studio Security, Landing Zones, Cosmos DB 5 Consistency Levels, AKS Pod Sandboxing (Kata), SQL Hyperscale, Virtual WAN Routing Intents, and FinOps. |
| **[Practice Set 5](quiz/az-305-practice-set-5.md)** | **50 Questions** | **Enterprise Case Studies & Advanced Operations:** Fabrikam Hybrid AD Case Study, Entra App Roles Claims, Administrative Units, Synthetic Transactions, Log Analytics Commitment Tiers & Private Endpoints. |
| **[Practice Set 6](quiz/az-305-practice-set-6.md)** | **50 Questions** | **Resource Optimization & Data Engineering:** Cost Budgets & Cross-Sub Tags, Azure Policy Scans, Storage Auth & WORM Immutability, Network Watcher Traffic Analytics, SQL Hyperscale/MI, Cosmos Multi-Master & Event Hubs Capture. |
| **[Practice Set 7](quiz/az-305-practice-set-7.md)** | **50 Questions** | **Data Protection, Resilient Storage & Global Workloads:** Dynamic Data Masking & Always Encrypted, ADLS Gen2 ACLs, Blob Point-in-Time Restore, Multi-User Authorization (MUA) for Backup, Gateway Load Balancer, and Container Apps. |
| **[Practice Set 8](quiz/az-305-practice-set-8.md)** | **50 Questions** | **Hybrid Networking, Cloud Ingress & Modern Compute:** ExpressRoute BGP Route Advertisement, Private DNS Resolver, Front Door WAF Rate-Limiting, APIM mTLS on AKS, Azure Migrate, and CycleCloud HPC. |
| **[Practice Set 9](quiz/az-305-practice-set-9.md)** | **50 Questions** | **Enterprise Architecture & Complex Migrations:** Fabrikam Case Study, Multi-Forest Entra Sync, SQL Host Disk Caching, Azure Batch MPI, On-Premises Data Gateway, Virtual WAN Multi-Hub & Redis Cache-Aside. |
| **[Case Study 1](quiz/az-305-case-study-1.md)** | **Case Study (22 Qs)** | **Contoso Pharmaceuticals:** Global Hybrid Modernization, Azure SQL Hyperscale Geo-Replication, Virtual WAN Secured Hubs, Front Door Premium Private Link, and WORM Storage. |
| **[Case Study 2](quiz/az-305-case-study-2.md)** | **Case Study (22 Qs)** | **Litware Financial Services:** Cloud-Native Core Banking, Cosmos DB Multi-Region Writes, Event Hubs Capture, Azure AI Foundry Zero-Trust, and Landing Zone Governance. |

---

## 🚀 Recommended Study Flow

```
┌────────────────────────────────────────────────────────────────────────┐
│                        AZ-305 PREPARATION ROADMAP                      │
├────────────────────────────────────────────────────────────────────────┤
│ 1. READ       ──► Review domain notes for each architectural topic     │
│ 2. EVALUATE   ──► Consult Azure Architecture Center decision guides    │
│ 3. PRACTICE   ──► Run through Practice Sets 1–9 under timed conditions │
│ 4. BENCHMARK  ──► Target 85%+ consistently across all 450 questions   │
└────────────────────────────────────────────────────────────────────────┘
```

---

## 🔗 Quick Navigation
- **Study Guide & Decision Trees:** [AZ-305 Master Study Plan](305-study-plan.md)
- **Practice Suite:** [AZ-305 Practice Quiz & Mock Tracker](quiz/az-305-quiz.md)
