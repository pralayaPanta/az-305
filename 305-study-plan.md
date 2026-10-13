---
tags:
  - studynote
  - certification
  - azure
  - architecture
  - az-305
series: AZ-305
created: 2026-07-25
updated: 2026-08-31
type: study-plan
---

# AZ-305: Designing Microsoft Azure Infrastructure Solutions — Master Study Plan

> **Target Credential:** Microsoft Certified: Azure Solutions Architect Expert
> **Exam Date:** 🟠 **Friday, October 16, 2026 at 10:00 AM AEST**
> **Booking Voucher:** `MSAIVKCJ5FBC` (AI Skills Fest R2)
> **Study Window:** August 31 – October 15, 2026 (6.5 Weeks / 46 Days)
> **Prerequisites:** ✅ AZ-104 Certified (valid through Mar 2027)

---

## 🎯 1. Exam Weighting & Candidate Strength Matrix

| Official Exam Domain | Weight | Baseline Strength | Effort % | Primary Focus Areas |
|---|:---:|:---:|:---:|---|
| **Domain 1: Identity, Governance & Monitoring** | **25–30%** | 🟢 Expert | 10% | CAF Landing Zones, Management Groups, Policy, Multi-workspace Log Analytics |
| **Domain 2: Design Data Storage Solutions** | **20–25%** | 🟡 Moderate | 🔴 35% | Cosmos DB 5 consistency levels, SQL DB vs. MI vs. Hyperscale, Blob tiers & WORM |
| **Domain 3: Business Continuity Solutions** | **15–20%** | 🟡 Moderate | 🔴 25% | ASR RTO/RPO, Backup Vault tiers, CRR, Storage redundancy (ZRS/GRS/GZRS) |
| **Domain 4: Design Infrastructure Solutions** | **30–35%** | 🟢 Strong | 🟠 30% | Compute decision tree, Load Balancers (Front Door vs. App Gateway), Virtual WAN |

---

## 📚 2. Study Resource Stack

These are the **exact resources** that map to every week in this plan:

### 🎥 Video (Primary Learning)
| Resource | Where | Use |
|---|---|---|
| **Pluralsight — AZ-305 Learning Path** | [pluralsight.com](https://www.pluralsight.com/paths/microsoft-azure-solutions-architect-az-305) | Core structured curriculum — one module per topic block |
| **John Savill — AZ-305 Study Cram** | [YouTube Playlist](https://www.youtube.com/playlist?list=PLlVtbbG169nHSnaP4ae33yQUI3zcmP5nP) | Whiteboard deep-dives for every architectural trade-off |
| **John Savill — AZ-305 Exam Cram v2** | [YouTube (3hr)](https://www.youtube.com/watch?v=vq9LuCM4YP4) | Final 4-day sprint consolidation video |

### 📖 Documentation (Reading)
| Resource | Where | Use |
|---|---|---|
| **Microsoft Learn — AZ-305 Official Path** | [learn.microsoft.com](https://learn.microsoft.com/en-us/credentials/certifications/azure-solutions-architect/) | Module-by-module reading tied to each week |
| **Azure Architecture Center** | [learn.microsoft.com/azure/architecture](https://learn.microsoft.com/en-us/azure/architecture/) | Decision trees: compute, data, network, messaging |
| **Azure Well-Architected Framework (WAF)** | [learn.microsoft.com/azure/well-architected](https://learn.microsoft.com/en-us/azure/well-architected/) | Referenced in [[Azure Well-Architected Framework]] |
| **Cloud Adoption Framework (CAF)** | [learn.microsoft.com/azure/cloud-adoption-framework](https://learn.microsoft.com/en-us/azure/cloud-adoption-framework/ready/landing-zone/) | Landing Zones (Domain 1) |
| **AZ-305 Study Guide — Charbel Nemnom** | [charbelnemnom.com](https://charbelnemnom.com/az-305-exam-study-guide/) | Community guide with free instructor-led video links |
| **AZ-305 GitHub Study Notes — AkashHiwale** | [github.com/AkashHiwale/AZ-305](https://github.com/AkashHiwale/AZ-305-Azure-Solutions-Architect-Expert-Study-Notes) | Community reference per topic |
| **AZ-305 GitHub Labs — timothywarner** | [github.com/timothywarner/az305](https://github.com/timothywarner/az305) | Hands-on lab scenarios |
| **AZ-305 GitHub www42** | [github.com/www42/AZ-305](https://github.com/www42/AZ-305) | Additional useful links & lab materials |

### 🧪 Practice & Quizzes
| Resource                                    | When                                           | Target                   |
| ------------------------------------------- | ---------------------------------------------- | ------------------------ |
| **MS Learn Official Practice Assessment**   | Week 1 baseline + Final Sprint                 | ≥85% in final sprint     |
| **Tutorials Dojo — AZ-305 Practice Exams**  | Weeks 1–5 (section-based, per domain)          | ≥75% per domain section  |
| **MeasureUp — AZ-305 Practice Test**        | Week 6 (timed full mocks)                      | ≥80% mock 1, ≥85% mock 2 |
| **Whizlabs — AZ-305 Practice Tests + Labs** | Optional — supplement Tutorials Dojo if needed | ≥75%                     |
|                                             |                                                |                          |

### ✍️ Vault Notes (Your Existing Files)
| Note File | Topic | Status |
|---|---|---|
| [[mscerts/az-305/Identity - Authentication\|Identity - Authentication.md]] | Domain 1.1 — Auth & Entra ID | Exists — update during Week 5 |
| [[mscerts/az-305/Monitoring - Solutions\|Monitoring - Solutions.md]] | Domain 1.3 — Azure Monitor & Log Analytics | Exists — update during Week 5 |
| [[Storage3 - relational database\|Storage - relational data.md]] | Domain 2.2 — Relational DB & SQL tiers | Exists — update during Week 2 |
| [[Business Cont - Backup & recovery\|Backup & recovery.md]] | Domain 3 — ASR, Azure Backup, CRR | Exists — update during Week 5 |
| [[mscerts/az-305/Azure Well-Architected Framework\|Azure Well-Architected Framework.md]] | WAF (all domains) | Exists — reference throughout |
| [[305-study-plan\|az-305-study.md]] | Master study plan | **You are here** |
| [[az-305-quiz\|az-305-quiz.md]] | Quiz tracker & benchmark log | Active — log errors weekly |

---

## 🔄 3. The 5-Pillar Daily Study Loop

Apply this exact sequence for **every topic block** in the weekly schedule below:

```
┌──────────────────────────────────────────────────────────────────────────────────────┐
│                          5-PILLAR ARCHITECTURAL STUDY ENGINE                         │
├──────────────────────────────────────────────────────────────────────────────────────┤
│ 1. 📺 WATCH  ──► Pluralsight module for topic + John Savill deep-dive video          │
│ 2. 📖 READ   ──► MS Learn module + Architecture Center decision guide                │
│ 3. ✍️ NOTE   ──► Synthesize trade-offs into the relevant vault note file             │
│ 4. 🧪 QUIZ   ──► Tutorials Dojo section test for the domain (target ≥75%)           │
│ 5. 🔁 REVIEW ──► Log all wrong answers in az-305-quiz.md with reason why            │
└──────────────────────────────────────────────────────────────────────────────────────┘
```

---

## 📖 4. Complete Syllabus — All Domains, Sub-Objectives & Sub-Topics

### Domain 1: Design Identity, Governance & Monitoring Solutions (25–30%)

**1.1 Design Authentication and Authorization Solutions**
- [ ] Microsoft Entra ID — tenants, custom domains, B2B Guest vs. B2C/External ID
- [ ] Conditional Access — Zero Trust architecture, Named Locations, Authentication Strengths
- [ ] Privileged Identity Management (PIM) for Azure RBAC & Entra ID roles
- [ ] Managed Identities — system-assigned vs. user-assigned; Service Principals for app auth
- [ ] Microsoft Entra Domain Services (Azure AD DS) vs. traditional AD DS on-prem integration
- [ ] App Registrations — OAuth2.0, OIDC, API permissions, client credentials flow
- [ ] Vault Note: [[mscerts/az-305/Identity - Authentication|Identity - Authentication.md]]

**1.2 Design a Governance Solution**
- [ ] Cloud Adoption Framework (CAF) — Enterprise-Scale / Azure Landing Zone architecture
- [ ] Management Group hierarchy — subscription vending, resource group structure
- [ ] Azure Policy — definitions, initiatives (built-in vs. custom), exclusions, remediation tasks
- [ ] Azure Blueprints vs. Policy Initiatives (deprecated path awareness)
- [ ] Resource Locks (CanNotDelete vs. ReadOnly) — when to apply at which scope
- [ ] Cost Management — Budgets, Tagging inheritance, Cost Allocation rules, Advisor recommendations
- [ ] Key Reference: [Azure Policy documentation](https://learn.microsoft.com/en-us/azure/governance/policy/)
- [ ] Key Reference: [CAF Landing Zones](https://learn.microsoft.com/en-us/azure/cloud-adoption-framework/ready/landing-zone/)
- [ ] Key Reference: [Resource naming & abbreviations](https://learn.microsoft.com/en-us/azure/cloud-adoption-framework/ready/azure-best-practices/resource-abbreviations)
- [ ] Key Reference: [Resource move support](https://learn.microsoft.com/en-us/azure/azure-resource-manager/management/move-support-resources)

**1.3 Design a Solution for Logging and Monitoring**
- [ ] Azure Monitor — Metrics vs. Logs, Activity Log vs. Resource Logs, Diagnostic Settings
- [ ] Log Analytics Workspace — centralized vs. decentralized, table-level retention, Dedicated Cluster
- [ ] Application Insights — distributed tracing, Live Metrics, Smart Detection, sampling
- [ ] Azure Monitor Alerts — metric alerts, log search alerts, action groups
- [ ] Microsoft Defender for Cloud — data ingestion design for multi-tenant monitoring
- [ ] Microsoft Sentinel — data connector architecture, multi-workspace / MSSP model
- [ ] KQL fundamentals: [KQL Learning Path](https://learn.microsoft.com/en-us/training/paths/kusto-query-language/) | [KQL for Monitoring](https://learn.microsoft.com/en-us/training/paths/analyze-monitoring-data-with-kql/)
- [ ] Vault Note: [[mscerts/az-305/Monitoring - Solutions|Monitoring - Solutions.md]]

---

### Domain 2: Design Data Storage Solutions (20–25%)

**2.1 Design a Data Storage Solution for Non-Relational Data**
- [ ] Azure Cosmos DB — 5 Consistency Models (Strong, Bounded Staleness, Session, Consistent Prefix, Eventual)
- [ ] Cosmos DB — APIs: SQL/Core, MongoDB, Cassandra, Gremlin, Table — when to choose each
- [ ] Cosmos DB — Partitioning: partition key selection, synthetic keys, multi-region writes (RU cost)
- [ ] Azure Blob Storage — Hot, Cool, Cold, Archive tiers; lifecycle management policy rules
- [ ] Azure Blob — Immutable Storage: time-based retention policies (WORM), legal holds
- [ ] Azure Blob — Object Replication, Versioning, Soft Delete, Change Feed
- [ ] Azure Files — SMB vs. NFS, Large File Shares (100 TiB), identity-based auth (Kerberos/AD DS)
- [ ] Azure NetApp Files — NFS/SMB, performance tiers (Standard/Premium/Ultra), sub-millisecond latency
- [ ] Azure Files vs. Azure NetApp Files — decision matrix (IOPS, compliance, latency)
- [ ] Storage Security — Customer-managed keys (CMK) via Key Vault, SAS tokens, Private Endpoints
- [ ] Key Reference: [Azure Cosmos DB documentation](https://learn.microsoft.com/en-us/azure/cosmos-db/)
- [ ] Key Reference: [Azure Files Kerberos auth](https://learn.microsoft.com/en-us/azure/storage/files/storage-files-identity-auth-hybrid-identities-enable)
- [ ] Key Reference: [Azure Messaging YouTube](https://www.youtube.com/watch?v=FVOhLqE9fzw)

**2.2 Design a Data Storage Solution for Relational Data**
- [ ] Database selection matrix — Azure SQL DB vs. SQL Managed Instance vs. SQL Server on Azure VMs
- [ ] SQL MI vs. SQL DB key differentiators — SQL Agent, CLR, cross-database queries, Linked Servers
- [ ] Azure SQL DB tiers — General Purpose, Business Critical, Hyperscale (up to 100 TB, rapid snapshots)
- [ ] Azure SQL Elastic Pools — shared DTU/vCore resources for variable multi-tenant SaaS workloads
- [ ] High Availability — Active Geo-Replication (read replicas) vs. Auto-Failover Groups (automatic DNS)
- [ ] Azure SQL DB HA architecture — [High availability for Azure SQL Database](https://learn.microsoft.com/en-us/azure/azure-sql/database/high-availability-sla)
- [ ] Azure SQL DB vs. SQL MI feature comparison — [Feature comparison](https://learn.microsoft.com/en-us/azure/azure-sql/database/features-comparison)
- [ ] Hyperscale tier — [Service tier documentation](https://learn.microsoft.com/en-us/azure/azure-sql/database/service-tier-hyperscale)
- [ ] Open-source relational — Azure Database for PostgreSQL / MySQL (Flexible Server, Zone-Redundant HA)
- [ ] Security — TDE (Transparent Data Encryption), Always Encrypted, Dynamic Data Masking, RBAC
- [ ] Vault Note: [[Storage3 - relational database|Storage - relational data.md]]

**2.3 Design Data Integration & Analytics Solutions**
- [ ] Azure Data Factory — Pipelines, Activities, Self-Hosted Integration Runtime (SHIR), Data Flows
- [ ] Azure Synapse Analytics — Serverless SQL Pool vs. Dedicated SQL Pool vs. Spark Pool
- [ ] Microsoft Fabric — relationship with Synapse, Lakehouse architecture
- [ ] Azure Data Lake Storage Gen2 — hierarchical namespace, POSIX ACLs, lifecycle management
- [ ] Azure Databricks — when to use over Synapse Spark
- [ ] Event-based ingestion — Event Hubs (streaming) vs. Stream Analytics (real-time transformation)
- [ ] Key Reference: [Azure IoT Reference Architecture](https://learn.microsoft.com/en-us/azure/architecture/reference-architectures/iot)
- [ ] Key Reference: [Integration Services](https://azure.microsoft.com/en-us/products/category/integration/)

---

### Domain 3: Design Business Continuity Solutions (15–20%)

**3.1 Design a Solution for Backup and Disaster Recovery**
- [ ] RTO (Recovery Time Objective) vs. RPO (Recovery Point Objective) — architecture implications
- [ ] Azure Site Recovery (ASR) — VM replication, Mobility Service agent, replication policies, test failover
- [ ] ASR — supported scenarios: Azure-to-Azure, on-premises Hyper-V/VMware to Azure
- [ ] Azure Backup — Recovery Services Vault vs. Backup Vault (which protects what)
- [ ] Backup tiering — Vault-Standard (operational) vs. Vault-Archive (long-term retention, lower cost)
- [ ] Cross-Region Restore (CRR) — requirements: GRS/GZRS vault, secondary region restore capability
- [ ] Soft Delete (accidental deletion protection) & Multi-User Authorization (MUA) for backups
- [ ] SQL DB backups — full/differential/log, point-in-time restore (PITR), long-term retention (LTR)
- [ ] Vault Note: [[Business Cont - Backup & recovery|Backup & recovery.md]]

**3.2 Design for High Availability**
- [ ] Storage Redundancy — LRS (3 copies, 1 region), ZRS (3 zones, 1 region), GRS (paired region), RA-GRS, GZRS, RA-GZRS
- [ ] VM Availability Sets — Fault Domains (rack separation) vs. Update Domains (patch sequencing)
- [ ] VM Availability Zones — 99.99% SLA, physical datacenter separation within region
- [ ] Multi-region active-active vs. active-passive patterns for web / app / database tiers
- [ ] Azure Front Door as global HA for HTTP workloads (automatic failover)
- [ ] SLA calculation — composite SLA = product of individual component SLAs

---

### Domain 4: Design Infrastructure Solutions (30–35%)

**4.1 Design a Compute Solution**
- [ ] Azure Compute Decision Tree — [Architecture Center: Choose an Azure compute service](https://learn.microsoft.com/en-us/azure/architecture/guide/technology-choices/compute-decision-tree)
- [ ] Virtual Machines — VM families (D, E, F, M, N series), spot VMs, reserved instances
- [ ] VM Scale Sets (VMSS) — Flexible orchestration vs. Uniform orchestration, autoscale policies
- [ ] Azure App Service — pricing tiers (Free/Shared/Basic/Standard/Premium/Isolated), deployment slots
- [ ] App Service Environments (ASE v3) — dedicated isolated infrastructure, internal vs. external
- [ ] App Service — VNet Integration (outbound) vs. Private Endpoints (inbound)
- [ ] Azure Kubernetes Service (AKS) — CNI vs. Kubenet, node pool types, cluster autoscaler, private cluster
- [ ] Azure Container Apps (ACA) — KEDA event-driven autoscaling, Dapr integration, microservices
- [ ] Container Apps vs. AKS — when to choose each (managed vs. control-plane access)
- [ ] Azure Functions — Consumption vs. Premium vs. Dedicated (App Service Plan) hosting
- [ ] Azure Logic Apps — Standard vs. Consumption, workflow orchestration
- [ ] Azure Batch — HPC parallel job processing (distinct from Functions/ACA)
- [ ] Key Reference: [Phippy's Field Guide to Wasm (container context)](https://www.youtube.com/watch?v=eFE6yGufDAA)

**4.2 Design an Application Architecture & Messaging**
- [ ] Event & Message routing decision matrix:
  - **Event Grid** — event-driven reactive architecture, push model, near-real-time (≤10ms)
  - **Event Hubs** — high-throughput streaming ingestion (millions of events/sec), Kafka-compatible
  - **Service Bus (Queues)** — guaranteed order, dead-letter queue, at-most-once/at-least-once delivery
  - **Service Bus (Topics/Subscriptions)** — pub/sub fan-out to multiple consumers
  - **Storage Queues** — simple, massive scale, no ordering guarantee
- [ ] Key Reference: [Azure Messaging — YouTube](https://www.youtube.com/watch?v=FVOhLqE9fzw)
- [ ] Key Reference: [Azure Messaging Conference Talk](https://www.youtube.com/watch?v=FVOhLqE9fzw)
- [ ] API Management (APIM) — policies (inbound/outbound/backend/on-error), self-hosted gateway, rate limiting, subscription keys
- [ ] Azure Key Vault — Access Policy model vs. Azure RBAC model, HSM-backed keys, key rotation, secrets & certificates
- [ ] Key Reference: [Bicep Playground](https://azure.github.io/bicep/) | [ARM/Bicep/Terraform reference](https://learn.microsoft.com/en-us/azure/templates/)

**4.3 Design Network Solutions**
- [ ] Load Balancing Selection Matrix:
  - **Azure Front Door** — global, Layer 7 (HTTP/HTTPS), WAF, CDN, SSL offload, geo-routing
  - **Application Gateway v2** — regional, Layer 7, URL path routing, cookie affinity, WAF
  - **Azure Traffic Manager** — global, DNS-based (Layer 4 equivalent), non-HTTP, multi-region failover
  - **Azure Load Balancer (Standard)** — regional, Layer 4 (TCP/UDP), ultra-low latency, HA ports
- [ ] Key Reference: [Azure Architecture Load Balancing Overview](https://learn.microsoft.com/en-us/azure/architecture/guide/technology-choices/load-balancing-overview)
- [ ] Hybrid Connectivity — ExpressRoute (Direct vs. Provider), FastPath, Private Peering vs. Microsoft Peering
- [ ] VPN Gateway — S2S, P2S, VNet-to-VNet; Active-Active vs. Active-Passive
- [ ] Virtual WAN (Standard Hub) — managed hub with integrated Firewall, ExpressRoute, VPN, SD-WAN
- [ ] Traditional Hub-and-Spoke — customer-managed hub VNet, when to use over Virtual WAN
- [ ] Network Security — Azure Firewall Standard/Premium (IDPS, TLS inspection), NSGs, ASGs
- [ ] Azure Bastion — managed RDP/SSH without public IPs; Standard vs. Basic SKU
- [ ] Private Access — Private Endpoints (PaaS into VNet), Private Link Service, Azure DNS Private Resolver
- [ ] Key Reference: [Entra External ID docs](https://learn.microsoft.com/en-us/entra/external-id/) (for app network auth patterns)
- [ ] Key Reference: [Naming rules for Azure resources](https://learn.microsoft.com/en-us/azure/azure-resource-manager/management/resource-name-rules)

**4.4 Design Migrations**
- [ ] Azure Migrate — Discovery (appliance-based), Assessment (readiness + right-sizing), Migration
- [ ] Azure Migrate — Server Migration (Hyper-V, VMware, Physical, AWS, GCP)
- [ ] Database Migration Service (DMS) — offline migration (tolerate downtime) vs. online migration (near-zero downtime)
- [ ] App Service Migration Assistant — web app lift-and-shift assessment
- [ ] WAF pillar cross-check — reliability and cost optimization for migrated workloads

---

## 🗓️ 5. Week-by-Week Execution Plan (Aug 31 – Oct 16)

> **Schedule overview — all 4 domains, each in their own block:**

| Block            | Dates           | Domain                                                    | Weight |
| ---------------- | --------------- | --------------------------------------------------------- | :----: |
| **Week 1**       | Aug 31 – Sep 6  | Domain 2.1 — Non-Relational Storage & Cosmos DB           | 20–25% |
| **Week 2**       | Sep 7 – Sep 13  | Domain 2.2 & 2.3 — Relational DBs + Data Integration      | 20–25% |
| **Week 3**       | Sep 14 – Sep 20 | Domain 4.1 & 4.2 — Compute + App Architecture & Messaging | 30–35% |
| **Week 4**       | Sep 21 – Sep 27 | Domain 4.3 & 4.4 — Networking + Migrations                | 30–35% |
| **Week 5**       | Sep 28 – Oct 2  | **Domain 3 — Business Continuity & High Availability**    | 15–20% |
| **Week 6**       | Oct 3 – Oct 7   | **Domain 1 — Identity, Governance & Monitoring**          | 25–30% |
| **Sim Block**    | Oct 8 – Oct 11  | Case Studies + MeasureUp Timed Mock Exams                 |   —    |
| **Final Sprint** | Oct 12 – Oct 15 | Exam Cram v2 + Flashcards + Rest                          |   —    |
| **EXAM**         | **Oct 16**      | 🎯 AZ-305 — 10:00 AM AEST                                 |   —    |

---

### 📅 Week 1 (Aug 31 – Sep 6) — Domain 2.1: Non-Relational Storage & Cosmos DB

**Syllabus Focus:** Cosmos DB consistency + APIs + partitioning, Blob tiers & lifecycle, Azure Files vs. NetApp Files, Storage security

| Day         | Activity                                                                                                                                                                   | Resource                                                                                                                                                      |
| ----------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Mon (Sep 1) | 🧪 Baseline MS Learn Practice Assessment — diagnostic only, no prep                                                                                                        | [MS Learn Assessment](https://learn.microsoft.com/en-us/credentials/certifications/exams/az-305/practice/assessment?assessment-type=practice&assessmentId=60) |
| Mon–Tue     | 📺 Pluralsight: *Designing Non-Relational Data Storage for Microsoft Azure*                                                                                                | Pluralsight AZ-305 Path                                                                                                                                       |
| Mon–Tue     | 📺 John Savill: Cosmos DB consistency models + API selection                                                                                                               | [Study Cram Playlist](https://www.youtube.com/playlist?list=PLlVtbbG169nHSnaP4ae33yQUI3zcmP5nP)                                                               |
| Wed         | 📖 MS Learn: *Design a data storage solution for non-relational data*                                                                                                      | MS Learn AZ-305 Path                                                                                                                                          |
| Wed         | 📖 Read: [Cosmos DB documentation](https://learn.microsoft.com/en-us/azure/cosmos-db/) — APIs, consistency, partitioning                                                   | Focus on all 5 APIs                                                                                                                                           |
| Thu         | 📖 Read: Blob tiers (Hot/Cool/Cold/Archive), Lifecycle policy rules, Immutable Storage (WORM), Object Replication                                                          | MS Learn Storage Modules                                                                                                                                      |
| Thu         | 📖 Read: [Azure Files Kerberos auth](https://learn.microsoft.com/en-us/azure/storage/files/storage-files-identity-auth-hybrid-identities-enable) + NetApp Files comparison | Domain 2.1 coverage                                                                                                                                           |
| Thu         | ✍️ Note: Cosmos DB 5 consistency matrix + API decision table                                                                                                               | Add to vault                                                                                                                                                  |
| Fri         | ✍️ Note: Blob tier + WORM + Azure Files vs. NetApp Files comparison table                                                                                                  | Add to vault                                                                                                                                                  |
| Sat         | 🧪 Tutorials Dojo: Domain 2 — Non-Relational Storage section test                                                                                                          | Target ≥75%                                                                                                                                                   |
| Sat         | 🔁 Log all wrong answers in [[az-305-quiz\|az-305-quiz.md]]                                                                                                 | Record rationale                                                                                                                                              |
| Sun         | 📖 Review: [AZ-305 Study Guide — Charbel Nemnom](https://charbelnemnom.com/az-305-exam-study-guide/) — Domain 2 section                                                    | Community guide                                                                                                                                               |
| Sun         | 📖 Review: [AkashHiwale GitHub Notes](https://github.com/AkashHiwale/AZ-305-Azure-Solutions-Architect-Expert-Study-Notes) — Non-Relational section                         | Community notes                                                                                                                                               |

---

### 📅 Week 2 (Sep 7 – Sep 13) — Domain 2.2 & 2.3: Relational DBs & Data Integration

**Syllabus Focus:** Azure SQL DB vs. MI vs. Hyperscale, Elastic Pools, Geo-Replication vs. Failover Groups, Synapse, ADF, Data Lake Gen2

| Day | Activity | Resource |
|---|---|---|
| Mon–Tue | 📺 Pluralsight: *Designing Relational Data Storage on Microsoft Azure* | Pluralsight AZ-305 Path |
| Mon–Tue | 📺 John Savill: Azure SQL DB vs. SQL MI vs. SQL VM deep-dive | [Study Cram Playlist](https://www.youtube.com/playlist?list=PLlVtbbG169nHSnaP4ae33yQUI3zcmP5nP) |
| Wed | 📖 Read: [Azure SQL DB vs. SQL MI feature comparison](https://learn.microsoft.com/en-us/azure/azure-sql/database/features-comparison) | SQL Agent, CLR, Linked Servers |
| Wed | 📖 Read: [Azure SQL Hyperscale service tier](https://learn.microsoft.com/en-us/azure/azure-sql/database/service-tier-hyperscale) | 100TB, rapid snapshots |
| Wed | 📖 Read: [SQL DB High Availability SLA](https://learn.microsoft.com/en-us/azure/azure-sql/database/high-availability-sla) | Geo-Replication vs. Failover Groups |
| Thu | 📺 Pluralsight: *Designing Data Integration Solutions for Microsoft Azure* | Pluralsight AZ-305 Path |
| Thu | 📖 MS Learn: Synapse Analytics, Data Factory, Data Lake Gen2 modules | MS Learn AZ-305 Path |
| Thu | 📖 Read: [IoT Reference Architecture](https://learn.microsoft.com/en-us/azure/architecture/reference-architectures/iot) + [Integration Services](https://azure.microsoft.com/en-us/products/category/integration/) | Domain 2.3 context |
| Fri | ✍️ Update: [[Storage3 - relational database\|Storage - relational data.md]] — SQL Feature Decision Matrix | GP vs. BC vs. Hyperscale |
| Fri | ✍️ Note: Synapse vs. Fabric vs. Databricks vs. ADF — use-case decision table | Add to vault |
| Sat | 🧪 Tutorials Dojo: Domain 2 — Relational Data section test | Target ≥75% |
| Sat | 🔁 Log wrong answers in [[az-305-quiz\|az-305-quiz.md]] | Record rationale |
| Sun | 📖 Review: [AkashHiwale GitHub Notes](https://github.com/AkashHiwale/AZ-305-Azure-Solutions-Architect-Expert-Study-Notes) — Relational & Integration | Community notes |
| Sun | 📖 Skim: [timothywarner/az305 Labs](https://github.com/timothywarner/az305) — Domain 2 lab scenarios | Hands-on context |

---

### 📅 Week 3 (Sep 14 – Sep 20) — Domain 4.1 & 4.2: Compute, Containers & App Architecture

**Syllabus Focus:** Compute Decision Tree (VMs, App Service, ASE, ACA, AKS, Functions, Batch), VMSS, Event Grid/Hubs/Service Bus, APIM, Key Vault

> ⚠️ **CONCURRENT MILESTONE:** Complete **AZ-500 Renewal** on MS Learn before **Sep 18** (🔴 Urgent — 18 days left)

| Day | Activity | Resource |
|---|---|---|
| Mon–Tue | 📺 Pluralsight: *Designing Compute Solutions for Microsoft Azure* | Pluralsight AZ-305 Path |
| Mon–Tue | 📺 John Savill: Compute architecture — App Service vs. ACA vs. AKS deep-dive | [Study Cram Playlist](https://www.youtube.com/playlist?list=PLlVtbbG169nHSnaP4ae33yQUI3zcmP5nP) |
| Mon | 📖 Read: [Azure Compute Decision Tree](https://learn.microsoft.com/en-us/azure/architecture/guide/technology-choices/compute-decision-tree) | Bookmark and annotate |
| Tue | ⚠️ Complete **AZ-500 Renewal** assessment on MS Learn | [MS Learn AZ-500 Renewal](https://learn.microsoft.com/en-us/credentials/certifications/azure-security-engineer/) |
| Tue | 📖 Read: VMSS Flexible vs. Uniform orchestration, AKS CNI vs. Kubenet, ACA KEDA scaling | MS Learn Compute Path |
| Wed | 📺 Pluralsight: *Designing Application Architecture for Microsoft Azure* | Pluralsight AZ-305 Path |
| Wed | 📺 John Savill: Event Grid vs. Event Hubs vs. Service Bus decision | [Study Cram Playlist](https://www.youtube.com/playlist?list=PLlVtbbG169nHSnaP4ae33yQUI3zcmP5nP) |
| Wed | 📖 Read: [Azure Messaging — YouTube](https://www.youtube.com/watch?v=FVOhLqE9fzw) + [Integration Services](https://azure.microsoft.com/en-us/products/category/integration/) | Messaging architecture |
| Thu | ✍️ Note: Compute decision matrix — VMs vs. App Service vs. ASE vs. ACA vs. AKS vs. Functions vs. Batch | New vault note |
| Thu | ✍️ Note: Messaging decision table — Event Grid vs. Event Hubs vs. Service Bus vs. Storage Queues | New vault note |
| Fri | 📖 Read: APIM policies, self-hosted gateway, Key Vault access models (RBAC vs. Access Policies) | MS Learn App Architecture Path |
| Sat | 🧪 Tutorials Dojo: Domain 4 — Compute section test | Target ≥75% |
| Sat | 🔁 Log wrong answers in [[az-305-quiz\|az-305-quiz.md]] | Record rationale |
| Sun | 📖 Review: [AkashHiwale Notes](https://github.com/AkashHiwale/AZ-305-Azure-Solutions-Architect-Expert-Study-Notes) — Compute & App Architecture section | Community notes |

---

### 📅 Week 4 (Sep 21 – Sep 27) — Domain 4.3 & 4.4: Networking & Migrations

**Syllabus Focus:** Load Balancing (Front Door vs. App Gateway vs. Traffic Manager vs. ALB), Virtual WAN vs. Hub-Spoke, ExpressRoute, Private Endpoints, Azure Migrate, DMS

| Day | Activity | Resource |
|---|---|---|
| Mon–Tue | 📺 Pluralsight: *Designing Network Solutions for Microsoft Azure* | Pluralsight AZ-305 Path |
| Mon | 📺 John Savill: Load Balancing decision — Front Door vs. App Gateway vs. Traffic Manager vs. ALB | [Study Cram Playlist](https://www.youtube.com/playlist?list=PLlVtbbG169nHSnaP4ae33yQUI3zcmP5nP) |
| Mon | 📖 Read: [Load Balancing Decision Guide](https://learn.microsoft.com/en-us/azure/architecture/guide/technology-choices/load-balancing-overview) | Global vs. Regional, L4 vs. L7 |
| Tue | 📺 John Savill: Virtual WAN vs. Hub-and-Spoke + ExpressRoute deep-dive | [Study Cram Playlist](https://www.youtube.com/playlist?list=PLlVtbbG169nHSnaP4ae33yQUI3zcmP5nP) |
| Tue | 📖 Read: ExpressRoute (Direct vs. Provider, FastPath, Private Peering), VPN Gateway, Private DNS Resolver | MS Learn Networking Path |
| Wed | 📖 Read: Azure Firewall Standard/Premium (IDPS), NSGs/ASGs, Azure Bastion, Private Endpoints | MS Learn Network Security |
| Wed | 📖 Read: [Naming rules for Azure resources](https://learn.microsoft.com/en-us/azure/azure-resource-manager/management/resource-name-rules) + [Resource move support](https://learn.microsoft.com/en-us/azure/azure-resource-manager/management/move-support-resources) | Governance/network context |
| Thu | 📺 Pluralsight: *Designing Migrations to Azure* | Pluralsight AZ-305 Path |
| Thu | 📖 Read: Azure Migrate — discovery appliance, assessment (readiness + sizing), server migration tools | MS Learn Migrations Path |
| Thu | 📖 Read: DMS offline vs. online migration, App Service Migration Assistant | MS Learn Migrations Path |
| Fri | ✍️ Note: Load Balancer selection matrix (Global vs. Regional, L4 vs. L7) | New vault note |
| Fri | ✍️ Note: Hybrid connectivity — ExpressRoute vs. S2S VPN vs. Virtual WAN decision table | New vault note |
| Sat | 🧪 Tutorials Dojo: Domain 4 — Networking section test | Target ≥75% |
| Sat | 🔁 Log wrong answers in [[az-305-quiz\|az-305-quiz.md]] | Record rationale |
| Sun | 📖 Review: [AkashHiwale Notes](https://github.com/AkashHiwale/AZ-305-Azure-Solutions-Architect-Expert-Study-Notes) — Networking section | Community notes |
| Sun | 📖 Review: [www42/AZ-305 Useful Links](https://github.com/www42/AZ-305) + [timothywarner/az305](https://github.com/timothywarner/az305) | Supplementary materials |

---

### 📅 Week 5 (Sep 28 – Oct 2) — Domain 3: Business Continuity & High Availability

**Syllabus Focus:** RTO/RPO design, Azure Site Recovery (ASR), Azure Backup (CRR, vault tiers, Soft Delete, MUA), Storage redundancy (LRS/ZRS/GRS/GZRS), VM Availability Zones vs. Sets, composite SLA

> ⚠️ **CONCURRENT MILESTONE:** Complete **MS-102 & MD-102 Renewals** on MS Learn before **Sep 29** (🔴 Urgent)

| Day | Activity | Resource |
|---|---|---|
| Mon (Sep 28) | ⚠️ Complete **MS-102 & MD-102 Renewals** on MS Learn | [MS Learn Renewals](https://learn.microsoft.com/en-us/credentials/certifications/) |
| Mon | 📺 Pluralsight: *Designing Business Continuity Solutions for Microsoft Azure* | Pluralsight AZ-305 Path |
| Mon | 📺 John Savill: Azure Site Recovery deep-dive (ASR scenarios, replication policies, test failover) | [Study Cram Playlist](https://www.youtube.com/playlist?list=PLlVtbbG169nHSnaP4ae33yQUI3zcmP5nP) |
| Tue | 📖 MS Learn: *Design business continuity solutions* modules | MS Learn AZ-305 Path |
| Tue | 📺 John Savill: Azure Backup tiering — Vault-Standard vs. Vault-Archive, CRR, Soft Delete, MUA | [Study Cram Playlist](https://www.youtube.com/playlist?list=PLlVtbbG169nHSnaP4ae33yQUI3zcmP5nP) |
| Tue | ✍️ Update: [[Business Cont - Backup & recovery\|Backup & recovery.md]] — ASR scenarios, CRR requirements, RTO/RPO decision table, backup vault types | Key vault note |
| Wed | 📖 Read: Storage redundancy (LRS, ZRS, GRS, RA-GRS, GZRS, RA-GZRS) — when to use each | MS Learn Storage Resilience |
| Wed | 📖 Read: VM Availability Sets (fault/update domains) vs. Availability Zones (99.99% SLA), composite SLA | MS Learn HA Path |
| Wed | 📖 Read: SQL DB PITR, LTR, full/differential/log backup design | MS Learn SQL Backup |
| Thu | ✍️ Note: Storage redundancy comparison table (all 6 options) | Cross-reference with cheat sheet Section 6D |
| Thu | ✍️ Note: BCDR decision flow — which scenario → ASR vs. Azure Backup vs. CRR | New vault note |
| Fri | 🧪 Tutorials Dojo: Domain 3 — Business Continuity section test | Target ≥75% |
| Fri | 🔁 Log wrong answers in [[az-305-quiz\|az-305-quiz.md]] | Record rationale |
| Fri | 📖 Review: [Charbel Nemnom AZ-305 Study Guide](https://charbelnemnom.com/az-305-exam-study-guide/) — Domain 3 section | Community guide |

---

### 📅 Week 6 (Oct 3 – Oct 7) — Domain 1: Identity, Governance & Monitoring

**Syllabus Focus:** Entra ID (B2B/B2C, Conditional Access, PIM, Managed Identities), CAF Landing Zones, Management Groups, Azure Policy, Azure Monitor, Log Analytics, Application Insights, KQL

> 💡 **Note:** This is your expert domain (🟢 90%+ baseline). Focus on **confirming architectural design decisions** rather than learning fundamentals from scratch. Fewer videos, more reading + note synthesis.

| Day | Activity | Resource |
|---|---|---|
| Sat Oct 3 | 📺 Pluralsight: *Designing Identity, Governance & Monitoring Solutions for Microsoft Azure* | Pluralsight AZ-305 Path |
| Sat Oct 3 | 📺 John Savill: CAF Landing Zones + Management Group hierarchy | [Study Cram Playlist](https://www.youtube.com/playlist?list=PLlVtbbG169nHSnaP4ae33yQUI3zcmP5nP) |
| Sun Oct 4 | 📺 John Savill: Azure Monitor architecture, Log Analytics workspace design | [Study Cram Playlist](https://www.youtube.com/playlist?list=PLlVtbbG169nHSnaP4ae33yQUI3zcmP5nP) |
| Sun Oct 4 | 📖 Read: [CAF Landing Zones](https://learn.microsoft.com/en-us/azure/cloud-adoption-framework/ready/landing-zone/) + [Azure Policy documentation](https://learn.microsoft.com/en-us/azure/governance/policy/) | Domain 1.2 |
| Mon Oct 5 | 📖 Read: [Entra External ID documentation](https://learn.microsoft.com/en-us/entra/external-id/) — B2B Guest vs. B2C/External ID | Domain 1.1 |
| Mon Oct 5 | 📖 Read: [MFA mandate for Azure sign-in](https://azure.microsoft.com/en-us/blog/announcing-mandatory-multi-factor-authentication-for-azure-sign-in/) + Conditional Access design | Domain 1.1 |
| Mon Oct 5 | 📖 Read: [Elevate access to manage subscriptions](https://learn.microsoft.com/en-us/azure/role-based-access-control/elevate-access-global-admin) + PIM for Azure RBAC | Domain 1.1 |
| Mon Oct 5 | ✍️ Update: [[mscerts/az-305/Identity - Authentication\|Identity - Authentication.md]] — Conditional Access, PIM, Managed Identities, App Registration patterns | Key vault note |
| Tue Oct 6 | 📖 MS Learn: *Design governance, identity, and monitoring* domain modules | MS Learn AZ-305 Path |
| Tue Oct 6 | 📖 Read: [KQL learning path](https://learn.microsoft.com/en-us/training/paths/kusto-query-language/) + [KQL for monitoring](https://learn.microsoft.com/en-us/training/paths/analyze-monitoring-data-with-kql/) — first 2 modules each | Domain 1.3 |
| Tue Oct 6 | ✍️ Update: [[mscerts/az-305/Monitoring - Solutions\|Monitoring - Solutions.md]] — Log Analytics workspace design (centralized vs. decentralized), App Insights, Defender for Cloud ingestion | Key vault note |
| Wed Oct 7 | ✍️ Note: CAF landing zone architecture + Management Group + Subscription vending decision flow | Add to vault |
| Wed Oct 7 | 🧪 Tutorials Dojo: Domain 1 — Identity, Governance & Monitoring section test | Target ≥75% |
| Wed Oct 7 | 🔁 Log wrong answers in [[az-305-quiz\|az-305-quiz.md]] | Record rationale |
| Wed Oct 7 | 📖 Review: [AkashHiwale Notes](https://github.com/AkashHiwale/AZ-305-Azure-Solutions-Architect-Expert-Study-Notes) — Identity & Governance section | Community notes |

---

### 📅 Sim Block (Oct 8 – Oct 11) — Case Studies & Full Timed Mock Exams

**Focus:** Case study methodology, architectural scenario drills, full timed MeasureUp simulations, MS Learn final assessment

| Day        | Activity                                                                                                                                                          | Resource                                                                                       |
| ---------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------- |
| Thu Oct 8  | 📺 John Savill: How to approach AZ-305 case studies (timing, tab strategy)                                                                                        | [Exam Cram Playlist](https://www.youtube.com/playlist?list=PLlVtbbG169nHSnaP4ae33yQUI3zcmP5nP) |
| Thu Oct 8  | 📖 Read: [[mscerts/az-305/Azure Well-Architected Framework\|Azure Well-Architected Framework.md]] — all 5 pillars (Reliability, Security, Cost, Performance, Ops) | In-vault reference                                                                             |
| Thu Oct 8  | 📖 Scan: [Azure Architecture Center reference architectures](https://learn.microsoft.com/en-us/azure/architecture/) — all key decision trees                      | Compute, storage, network                                                                      |
| Fri Oct 9  | 🧪 **MeasureUp Full Timed Mock Exam 1** (full exam conditions — no pausing)                                                                                       | Target: ≥80%                                                                                   |
| Fri Oct 9  | 🔁 Detailed error analysis — document architectural rationale for every wrong answer                                                                              | [[az-305-quiz\|az-305-quiz.md]]                                                 |
| Sat Oct 10 | 📖 Targeted re-study: weakest domains from Mock 1 — revisit relevant vault notes                                                                                  | All vault note files                                                                           |
| Sat Oct 10 | 📖 Skim: [timothywarner/az305 labs](https://github.com/timothywarner/az305) — case study lab scenarios                                                            | Lab context                                                                                    |
| Sat Oct 10 | 🧪 **MeasureUp Full Timed Mock Exam 2** (full exam conditions)                                                                                                    | Target: ≥85%                                                                                   |
| Sat Oct 10 | 🔁 Error analysis — confirm weak domains are resolved                                                                                                             | [[az-305-quiz\|az-305-quiz.md]]                                                 |
| Sun Oct 11 | 🧪 **MS Learn Official Practice Assessment** — second run                                                                                                         | Target ≥85%                                                                                    |
| Sun Oct 11 | 📖 Skim all 4 domain objective checklists in Section 4 — tick off confirmed topics                                                                                | This file                                                                                      |
| Sun Oct 11 | 📖 Review all decision cheat sheets in Section 6                                                                                                                  | This file                                                                                      |

---

### 📅 Final Sprint (Oct 12 – Oct 15) — Cram & Readiness Validation

| Day | Activity |
|---|---|
| **Mon Oct 12** | 📺 John Savill: [AZ-305 Exam Cram v2](https://www.youtube.com/watch?v=vq9LuCM4YP4) — full 3 hours at 1.25× speed. Pause only on uncertain topics. |
| **Tue Oct 13** | 🃏 Flashcard blitz: Cosmos DB consistency levels, SQL MI vs. SQL DB decision, load balancer matrix, storage redundancy table, BCDR vault types. Review [[az-305-quiz\|az-305-quiz.md]] error log only. |
| **Wed Oct 14** | 📖 Light re-read: all 4 domain checklists in Section 4. Case study timing strategy: 25 min per case study, ~1 min per standalone question. |
| **Thu Oct 15** | 🧘 Short skim of cheat sheets in Section 6. Pack exam setup (webcam, ID, quiet room). No cramming after 8 PM. Early rest. |
| **Fri Oct 16** | 🎯 **EXAM DAY — 10:00 AM AEST** — AZ-305: Designing Microsoft Azure Infrastructure Solutions (Pearson VUE Online Proctored). Voucher: `MSAIVKCJ5FBC` |

---



## 🧠 6. Core Architectural Decision Cheat Sheets

### A. Cosmos DB 5 Consistency Levels
```
[Strong] ──► [Bounded Staleness] ──► [Session (Default)] ──► [Consistent Prefix] ──► [Eventual]
  ▲                                         ▲                                               ▲
Most consistent / highest cost       Read-your-own-writes per session            Lowest latency / cost
Single-region writes only            Best for e-commerce, shopping carts         Out-of-order reads OK
```

### B. Azure SQL — Service Selection Matrix
| Need | Answer |
|---|---|
| Full SQL Server compatibility (SQL Agent, CLR, Linked Servers, cross-DB queries) | **Azure SQL Managed Instance** |
| Cloud-native, up to 100 TB, instant backups & snapshots | **Azure SQL Database — Hyperscale** |
| Multi-tenant SaaS, variable/unpredictable load, shared compute | **Azure SQL Database — Elastic Pool** |
| Standard OLTP, 99.99% SLA, Business Critical for in-memory OLTP | **Azure SQL Database — Business Critical** |
| Global NoSQL, schema-flexible, <10ms SLA, multi-region writes | **Azure Cosmos DB** |
| PB-scale analytics & data warehousing | **Azure Synapse Analytics / Microsoft Fabric** |
| Full control over OS, SQL version, configuration | **SQL Server on Azure VM** |

### C. Load Balancing & Traffic Routing Selection Matrix
| Need | Answer |
|---|---|
| Global, HTTP/HTTPS, WAF, CDN, SSL offload, geo-routing | **Azure Front Door** |
| Regional, HTTP/HTTPS, URL path routing, WAF, cookie affinity | **Application Gateway v2** |
| Global, DNS-based, non-HTTP, multi-region failover, weighted routing | **Azure Traffic Manager** |
| Regional, TCP/UDP Layer 4, ultra-low latency, HA ports | **Azure Load Balancer (Standard)** |

### D. Storage Redundancy Quick Reference
| Tier | Copies | Zone-Resilient? | Region-Resilient? | Read Access in Secondary? |
|:---:|:---:|:---:|:---:|:---:|
| LRS | 3 in 1 zone | ❌ | ❌ | ❌ |
| ZRS | 3 across zones | ✅ | ❌ | ❌ |
| GRS | 3+3 across regions | ❌ | ✅ | ❌ |
| RA-GRS | 3+3 across regions | ❌ | ✅ | ✅ |
| GZRS | 3 zones + 3 in secondary | ✅ | ✅ | ❌ |
| RA-GZRS | 3 zones + 3 in secondary | ✅ | ✅ | ✅ |

### E. Messaging Service Decision Matrix
| Trigger | Service |
|---|---|
| React to Azure resource events (blob created, VM started) | **Event Grid** |
| High-throughput telemetry/streaming ingestion (IoT, clickstreams) | **Event Hubs** |
| Reliable message delivery, ordering, dead-letter, FIFO | **Service Bus (Queues)** |
| Pub/Sub fan-out to multiple independent consumers | **Service Bus (Topics/Subscriptions)** |
| Simple queue, massive scale, no ordering needed | **Azure Storage Queues** |

### F. Compute Selection Matrix
| Workload | Best Service |
|---|---|
| Full VM control, custom OS/software | **Azure Virtual Machines** |
| Lift-and-shift web apps, PaaS managed runtime | **Azure App Service** |
| High isolation for App Service (compliance, dedicated infra) | **App Service Environment (ASE v3)** |
| Microservices, event-driven scaling, no cluster management | **Azure Container Apps (ACA)** |
| Kubernetes with full cluster control, GPU, complex networking | **Azure Kubernetes Service (AKS)** |
| Event-triggered, short-duration, serverless execution | **Azure Functions** |
| HPC parallel batch processing (rendering, simulation) | **Azure Batch** |
| Stateful workflow orchestration | **Azure Logic Apps** |

---

## 🔗 7. Quick Navigation
- **Microsoft Certifications Index:** [[mscerts/mscert-readme|mscert-readme]]
- **Quiz Tracker & Error Log:** [[az-305-quiz|az-305-quiz.md]]
- **Identity & Auth Notes:** [[mscerts/az-305/Identity - Authentication|Identity - Authentication.md]]
- **Monitoring Notes:** [[mscerts/az-305/Monitoring - Solutions|Monitoring - Solutions.md]]
- **Storage Relational Notes:** [[Storage3 - relational database|Storage - relational data.md]]
- **Backup & Recovery Notes:** [[Business Cont - Backup & recovery|Backup & recovery.md]]
- **WAF Reference:** [[mscerts/az-305/Azure Well-Architected Framework|Azure Well-Architected Framework.md]]
- **Active Projects:** [[PROJECTS]] · **Memory:** [[MEMORY]]
- **Skills:** [[.agents/skills/cert-roadmap/SKILL|cert-roadmap skill]] · [[.agents/skills/exam-prep/SKILL|exam-prep skill]]
