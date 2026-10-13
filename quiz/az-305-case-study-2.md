---
title: "AZ-305 Case Study 2 — Litware Financial Services"
tags: [mscerts, az-305, case-study, azure, architecture, cosmosdb, event-hubs, ai-foundry, landing-zones, finops]
created: 2026-10-09
type: case-study
exam: AZ-305
total-questions: 22
---

# AZ-305 Case Study 2 — Litware Financial Services

```
Format: Full case study · 22 questions · Hot Area, Solution Series, Drag & Drop, Multi-Select, Multiple Choice
```

> [!warning] AI-Generated Study Material
> Synthetic practice scenario modelled on AZ-305 objectives. Not an official Microsoft exam item. Verify service limits against [Microsoft Learn](https://learn.microsoft.com/en-us/credentials/certifications/exams/az-305/).

> [!info] Case Study Instructions
> This is a case study. Case studies are not timed separately; you can use as much exam time as you would like to complete each case. To answer the questions, you must reference information provided in the case study. **Answer every question based only on the information in this case study.**
>
> Some questions in the **Solution Series** present a unique solution that might meet the stated goals. After you answer a question in that section, you will NOT be able to return to it.

---

## 📑 Case Study Dossier

### Overview

Litware Financial Services is a digital retail bank headquartered in New York, with offices in London and Tokyo. Litware processes approximately 80 million payment transactions per day from point-of-sale (POS) terminals, mobile apps, and partner banks.

Litware is building **Project Sentinel**, a new core ledger and real-time fraud scoring platform on Azure.

---

### Existing Environment

#### Applications and Data

- The current ledger runs on a sharded on-premises Oracle database in a New Jersey datacenter.
- An on-premises Apache Kafka cluster ingests transaction events. More than 300 Java producer applications (POS gateways, mobile back ends, and partner integrations) use Kafka client libraries. The Kafka cluster experiences backpressure during seasonal peaks.
- Settlement instructions are exchanged through IBM MQ. Instructions for the same account must be processed in order, and duplicate submissions occur when partner systems retry.
- Fraud models run as nightly batch jobs. Fraud is often detected hours after settlement.

#### Azure Environment

- Litware has an Azure Enterprise Agreement and a Microsoft Entra tenant named `litwarebank.com`.
- All existing Azure resources are deployed to a single subscription named Prod-Shared. No management groups exist below the Tenant Root Group.
- Tagging is inconsistent. A recent cost review found significant overspend caused by unattached managed disks and oversized resources.
- Developers deploy infrastructure by using Bicep templates in GitHub Actions workflows.
- The New Jersey datacenter connects to the East US region by using ExpressRoute. On-premises name resolution is provided by Windows Server DNS servers.
- The Java microservices have no distributed tracing.

---

### Requirements

#### Planned Changes

- Implement the Azure landing zone architecture. Create platform subscriptions named **Identity**, **Management**, and **Connectivity**, application landing zone subscriptions, and sandbox subscriptions.
- Deploy the following Project Sentinel components:
  - An Azure Cosmos DB for NoSQL account with **multi-region writes** in East US, North Europe, and Japan East
  - Microservices hosted in Azure Container Apps
  - Azure Event Hubs for event ingestion
  - Azure OpenAI models in Azure AI Foundry for fraud scoring
  - An API layer for POS terminals

#### Business Requirements

- Minimize costs and administrative effort.
- The code of the Kafka producer applications must NOT be modified.

#### Ledger Requirements

- Reads and writes must have single-digit millisecond latency in every region. Writes must be accepted in all three regions.
- After a transfer, customers must immediately see their updated balance in the same app session. Other readers, such as reporting services, can tolerate replication lag.
- Most queries retrieve transactions for a single account. Several corporate accounts generate more than 30 GB of transaction data per year.
- Concurrent updates to customer profile documents in different regions must be merged at the field level. No update must be discarded in its entirety.
- Accidentally deleted or corrupted data must be restorable to any point in time within the last 30 days.
- Analysts must run complex aggregations over ledger data in near real time without consuming provisioned throughput and without building ETL pipelines.
- Microservices must access Cosmos DB without keys. Key-based authentication must be disabled.
- The workload requires a baseline of 40,000 RU/s 24 hours a day. During several weeks each year, demand rises unpredictably to 150,000 RU/s.

#### Ingestion Requirements

- Ingest up to 150,000 events per second at peak. The average event size is 250 bytes.
- Raw events must be archived automatically to Azure Data Lake Storage Gen2 without writing custom code.

#### Settlement Requirements

- Settlement instructions for the same account must be processed in order.
- Duplicate instructions submitted within 10 minutes must be discarded automatically.
- Message processing must support transactions.

#### Microservice Requirements

- Production microservices must scale based on the number of unprocessed events and must never run fewer than two replicas.
- Non-production microservices must scale to zero when idle.
- Provide end-to-end distributed tracing across microservices by using vendor-neutral instrumentation.

#### Fraud Scoring Requirements

- Azure OpenAI must be reachable only through private IP addresses from the Container Apps virtual network. No public endpoint must be available.
- API keys must NOT be used.

#### POS API Requirements

- POS terminals must authenticate by using client certificates (mutual TLS).
- Requests must be protected against OWASP Top 10 attacks.
- Each terminal must be rate limited individually.
- Back-end services must NOT be reachable from the internet.

#### Network Requirements

- All outbound internet traffic from application landing zones must be inspected by using TLS inspection, intrusion detection and prevention (IDPS), and FQDN filtering.
- On-premises applications must resolve the private endpoint FQDNs of Azure PaaS services. No DNS virtual machines must be deployed to Azure.

#### Security and Compliance Requirements

- Cosmos DB and Azure Storage must be encrypted by using customer-managed keys stored in a **single-tenant**, fully managed HSM validated to FIPS 140-3 Level 3. Deleted keys must be recoverable during the retention period and must NOT be purgeable.
- Every resource in the application landing zones must send diagnostic logs to a central Log Analytics workspace in the Management subscription. Diagnostic settings must be configured automatically when resources are created. Deployments that omit diagnostic settings must NOT fail. Existing resources must also be remediated.
- Resources must automatically inherit the `CostCenter` and `Environment` tags from their resource group. Deployments must NOT be blocked.
- The finance team must be notified when a subscription reaches 80 percent of its monthly budget.

---

## ❓ Questions

### Question 1

You need to recommend a default consistency level for the Cosmos DB account that meets the ledger requirements.

What should you recommend?

A) Strong  
B) Bounded staleness  
C) Consistent prefix  
D) Session  

> [!faq]- Show Answer & Explanation
> **Answer: D** — Session consistency guarantees **read-your-own-writes** within a client session, so a customer sees their new balance immediately. Other sessions may see replication lag, which the requirements allow. It supports multi-region writes with single-digit millisecond latency.
>
> - *A:* Strong consistency **can't be used with multi-region writes**.
> - *B:* Bounded staleness reads cost twice as many RUs, and the extra guarantee isn't needed.
> - *C:* Consistent prefix guarantees ordering but **not** read-your-writes.
---

### Question 2 — Hot Area

You need to recommend an ingestion solution that meets the ingestion requirements and the business requirements.

**Answer Area**

| | Options |
|---|---|
| **Event Hubs tier:** | Basic · Standard · Premium · Dedicated |
| **Producer connectivity:** | Rewrite the producers by using the Event Hubs SDK · Run Kafka MirrorMaker on Azure VMs · Point the producers to the Event Hubs Kafka endpoint by changing configuration only · Publish to Azure Event Grid |

> [!faq]- Show Answer & Explanation
> **Box 1: Premium** — The arithmetic: 150,000 events/s × 250 bytes ≈ 37.5 MB/s.
> - *Basic:* no Kafka support and no Capture.
> - *Standard:* a throughput unit allows 1 MB/s **or 1,000 events/s**, and there's a maximum of 40 TUs (40,000 events/s), which can't handle 150,000 events/s.
> - *Premium:* supports Kafka, Capture, and dedicated resources at this scale.
> - *Dedicated:* would also work but is over-provisioned and more expensive ("minimize costs").
>
> **Box 2: Kafka endpoint, configuration only** — Event Hubs exposes a Kafka-compatible endpoint on port 9093. Producers change only the bootstrap server and SASL settings, **not the code**. Rewriting violates the business requirement, and MirrorMaker adds VMs to manage.
---

### Question 3

You need to recommend a solution for archiving raw events that meets the ingestion requirements.

What should you recommend?

A) An Azure Stream Analytics job that writes to Data Lake Storage Gen2  
B) An Azure Functions app with an Event Hubs trigger  
C) Event Hubs Capture configured with a Data Lake Storage Gen2 destination  
D) An Azure Data Factory pipeline that runs every five minutes  

> [!faq]- Show Answer & Explanation
> **Answer: C** — Event Hubs Capture is a built-in, **no-code** feature that writes event batches to Blob Storage or ADLS Gen2 in Avro format, based on time or size windows. Stream Analytics needs a query and job management. Functions needs custom code. Data Factory can't read directly from an event stream in this way.
---

### Question 4

You need to recommend a messaging service for settlement instructions.

What should you recommend?

A) Event Hubs Premium with a dedicated consumer group  
B) Azure Queue Storage  
C) Event Grid namespace topics  
D) Service Bus Premium queues with sessions and duplicate detection enabled  

> [!faq]- Show Answer & Explanation
> **Answer: D** — Each requirement maps to a Service Bus feature:
> - *Ordering per account:* message sessions (`SessionId = accountId`) give FIFO processing per session.
> - *Duplicates:* duplicate detection with a 10-minute window discards repeats by `MessageId`.
> - *Transactions:* Service Bus supports atomic transactions.
>
> Event Hubs (A) is for telemetry streaming and has no duplicate detection or transactions. Queue Storage (B) has no ordering guarantee. Event Grid (C) is for event notification.
---

### Question 5

You need to recommend a partition key strategy for the ledger transactions container.

What should you recommend?

A) `/accountId`  
B) `/transactionId`  
C) A hierarchical partition key of `/accountId` and then `/yearMonth`  
D) `/transactionDate`  

> [!faq]- Show Answer & Explanation
> **Answer: C** — A logical partition is limited to **20 GB**. `/accountId` alone gives efficient single-account queries, but corporate accounts that generate more than 30 GB a year would exceed the limit. **Hierarchical partition keys** (up to three levels) let one account span several logical partitions. Queries filtered on `accountId` are still routed only to the relevant physical partitions, not fanned out across the whole container.
>
> - *A:* fails the 20 GB limit for corporate accounts.
> - *B:* every account query would become a cross-partition fan-out.
> - *D:* creates a hot partition for the current date.
---

### Question 6

You need to recommend a conflict resolution policy for the customer profile container.

What should you recommend?

A) A custom conflict resolution policy that uses a merge stored procedure  
B) Last writer wins (LWW) by using the system `_ts` property  
C) Last writer wins (LWW) by using a custom numeric `/clientTimestamp` path  
D) Configure the account to use Strong consistency  

> [!faq]- Show Answer & Explanation
> **Answer: A** — The deciding phrase is "**merged at the field level, no update discarded in its entirety**." Both LWW options (B and C) pick **one whole document** as the winner and discard the other. A custom policy runs a registered stored procedure that receives the conflicting versions and can merge fields. Strong consistency (D) isn't supported with multi-region writes.
>
> > [!tip] Exam pattern
> > Choose LWW with a custom path when the stem says "the version with the highest business timestamp wins". Choose custom when it says "merge".
---

### Question 7

You need to recommend a solution for the analysts' aggregation queries that meets the ledger requirements.

What should you recommend?

A) Use the Cosmos DB change feed with an Azure Functions app that writes to Azure SQL Database.  
B) Run an Azure Data Factory copy pipeline every hour.  
C) Enable the Cosmos DB integrated cache with a dedicated gateway.  
D) Configure Mirroring for Azure Cosmos DB in Microsoft Fabric.  

> [!faq]- Show Answer & Explanation
> **Answer: D** — Fabric Mirroring continuously replicates Cosmos DB data into OneLake in Delta format. It's near real time, needs **no ETL**, and analytical queries run on Fabric compute **without consuming the container's provisioned RU/s**. (Azure Synapse Link for Cosmos DB was the earlier zero-ETL approach. Microsoft now steers new projects to Fabric Mirroring.)
>
> - *A:* reading the change feed consumes RUs and requires a custom pipeline.
> - *B:* this is ETL, and it consumes RUs.
> - *C:* the integrated cache speeds up point reads and queries but runs analytics against the transactional store.
---

### Question 8

You need to recommend a backup configuration for the Cosmos DB account that meets the ledger requirements.

What should you recommend?

A) Periodic backup with a 1-hour interval and 720-hour retention  
B) Continuous backup with the 7-day retention tier  
C) Continuous backup with the 30-day retention tier  
D) A daily Data Factory export to geo-redundant storage  

> [!faq]- Show Answer & Explanation
> **Answer: C** — "Any point in time within the last 30 days" requires **continuous backup (30-day tier)**, which supports self-service point-in-time restore at one-second granularity.
>
> - *A:* periodic backup restores only to discrete snapshots, through a support request.
> - *B:* the 7-day tier is too short.
> - *D:* a daily export gives a 24-hour granularity.
---

### Question 9 — Hot Area

You need to recommend an authentication solution for microservice access to Cosmos DB that meets the ledger requirements.

**Answer Area**

| | Options |
|---|---|
| **Identity:** | A service principal with a client secret · A user-assigned managed identity assigned to the container apps · A Cosmos DB primary key stored in Azure Key Vault |
| **Authorization:** | The Contributor role · The DocumentDB Account Contributor role · The Cosmos DB Built-in Data Contributor role · The Cosmos DB Account Reader role |

> [!faq]- Show Answer & Explanation
> **Box 1: User-assigned managed identity** · **Box 2: Cosmos DB Built-in Data Contributor**
>
> Cosmos DB has its own **data plane RBAC**, separate from Azure RBAC. The *Contributor* and *DocumentDB Account Contributor* roles are **control plane** roles: they manage the account but don't grant data access when keys are disabled. Assign the data plane role (`00000000-0000-0000-0000-000000000002`) to the managed identity, then set `disableLocalAuth = true` to block key-based authentication. A key in Key Vault is still key-based authentication.
---

### Question 10 — Hot Area

You need to recommend a throughput configuration for the ledger that meets the requirements and minimizes cost.

**Answer Area**

| | Options |
|---|---|
| **Throughput mode:** | Serverless · Standard (manual) provisioned throughput · Autoscale provisioned throughput |
| **Cost optimization:** | An Azure savings plan for compute · Azure Cosmos DB reserved capacity · Azure Hybrid Benefit |

> [!faq]- Show Answer & Explanation
> **Box 1: Autoscale** — Peaks are **unpredictable**, and administrative effort must be minimized. With an autoscale max of 150,000 RU/s, throughput scales instantly between 15,000 RU/s (10% of the max) and 150,000 RU/s, and you're billed for the highest RU/s reached in each hour.
> - *Serverless:* single-region only, so it's incompatible with multi-region writes.
> - *Manual throughput:* needs scripting to scale for unpredictable peaks.
>
> **Box 2: Cosmos DB reserved capacity** — A 1- or 3-year commitment on the steady 40,000 RU/s baseline gives significant discounts, and reserved capacity applies to autoscale throughput. A *savings plan for compute* doesn't cover Cosmos DB RU/s. *Azure Hybrid Benefit* applies to Windows Server and SQL Server licensing.
---

### Question 11 — Multi-Select

You need to recommend a configuration for Azure OpenAI that meets the fraud scoring requirements.

Which **three** actions should you include? Each correct answer presents part of the solution.

**NOTE:** Each correct selection is worth one point.

A) Create a private endpoint for the Azure OpenAI resource in a subnet that is reachable from the Container Apps virtual network.  
B) Store the Azure OpenAI API key as a Key Vault secret and reference it from the container apps.  
C) Set public network access on the Azure OpenAI resource to Disabled.  
D) Assign the Cognitive Services OpenAI User role to the managed identity of the container apps.  
E) Publish the Azure OpenAI endpoint through an Azure API Management instance that has an external virtual IP address.  

> [!faq]- Show Answer & Explanation
> **Answer: A, C, D**
> - **A:** gives Azure OpenAI a private IP address that's reachable from the Container Apps VNet (together with the `privatelink.openai.azure.com` DNS zone).
> - **C:** removes the public endpoint entirely.
> - **D:** enables keyless Entra ID authentication with least privilege (inference only).
>
> *B* still uses an API key. *E* creates a public endpoint.
---

### Question 12 — Hot Area

You need to configure scaling for the production event-processing container apps.

**Answer Area**

| | Options |
|---|---|
| **Scale rule:** | HTTP concurrency · CPU utilization · Custom KEDA scaler of type `azure-eventhub` · TCP connections |
| **Minimum replicas:** | 0 · 1 · 2 |

> [!faq]- Show Answer & Explanation
> **Box 1: Custom KEDA `azure-eventhub` scaler** · **Box 2: 2**
>
> The `azure-eventhub` scaler measures **unprocessed events** (consumer group lag compared with checkpoints in Blob Storage) and adds replicas based on `unprocessedEventThreshold`. Event consumers don't receive HTTP requests, so HTTP scaling never triggers, and CPU is a lagging signal. A minimum of **2** meets "never fewer than two replicas". For **non-production** apps, set `minReplicas = 0` so they scale to zero.
---

### Question 13 — Hot Area

You need to recommend an ingress solution for the POS API that meets the POS API requirements.

**Answer Area**

| | Options |
|---|---|
| **Internet-facing entry point:** | Azure Load Balancer Standard · Azure Front Door Standard · Azure Application Gateway WAF_v2 with mutual authentication · Azure Firewall with DNAT rules |
| **Per-terminal rate limiting:** | NSG rules · Azure API Management in internal virtual network mode with a `rate-limit-by-key` policy · Azure DDoS Network Protection · Container Apps ingress IP restrictions |

> [!faq]- Show Answer & Explanation
> **Box 1: Application Gateway WAF_v2 with mutual authentication** · **Box 2: API Management (internal mode) with `rate-limit-by-key`**
>
> Application Gateway v2 supports **mTLS** (validating client certificates against a trusted CA chain) and runs OWASP managed rule sets. APIM deployed in **internal** VNet mode sits behind it, with no public exposure. The `rate-limit-by-key` policy can key on the client certificate thumbprint or terminal ID, which gives **per-terminal** limits. The back ends remain private.
>
> Load Balancer and Azure Firewall have no WAF or mTLS. Front Door Standard can't reach private back ends. NSGs and DDoS Protection can't rate limit per client identity.
---

### Question 14 — Hot Area

You need to recommend an outbound traffic solution that meets the network requirements.

**Answer Area**

| | Options |
|---|---|
| **Azure Firewall SKU:** | Basic · Standard · Premium |
| **Route traffic by using:** | An NSG rule that denies the Internet service tag · A NAT gateway on each subnet · A UDR for `0.0.0.0/0` with the firewall private IP as the next hop · Service endpoints |

> [!faq]- Show Answer & Explanation
> **Box 1: Premium** · **Box 2: UDR `0.0.0.0/0` → firewall private IP (next hop type: Virtual appliance)**
>
> **TLS inspection and signature-based IDPS** are Premium-only features; Standard provides FQDN filtering and threat intelligence. The default route UDR on the spoke subnets forces all egress through the hub firewall in the Connectivity subscription.
>
> - *NAT gateway:* provides SNAT only, with no inspection.
> - *NSG:* blocks traffic but can't inspect it or filter by FQDN.
---

### Question 15

You need to recommend a name resolution solution that meets the network requirements.

What should you recommend?

A) Deploy Windows Server DNS virtual machines in the Connectivity subscription and configure conditional forwarders.  
B) Create a public DNS zone with A records for each private endpoint.  
C) Host the `privatelink.*` Private DNS zones centrally in the Connectivity subscription, link them to the hub VNet, deploy an Azure DNS Private Resolver inbound endpoint, and configure the on-premises DNS servers to conditionally forward to it.  
D) Configure hosts files on the on-premises application servers.  

> [!faq]- Show Answer & Explanation
> **Answer: C** — On-premises servers can't query Azure DNS (`168.63.129.16`) directly. The **DNS Private Resolver inbound endpoint** gives them a private IP address in the hub to forward to, so no DNS VMs are needed. Centralized `privatelink.*` zones (typically populated by an Azure Policy DeployIfNotExists rule) avoid duplicating zones across landing zones.
>
> - *A:* violates "no DNS VMs".
> - *B:* exposes private IP addresses in public DNS, an anti-pattern.
> - *D:* unmanageable.
---

### Question 16

You need to recommend a key management solution that meets the security and compliance requirements.

What should you recommend?

A) Azure Key Vault Standard with soft delete  
B) Azure Key Vault Premium with purge protection  
C) Azure Dedicated HSM  
D) Azure Key Vault Managed HSM with purge protection  

> [!faq]- Show Answer & Explanation
> **Answer: D** — Managed HSM is a **single-tenant**, fully managed HSM pool validated to FIPS 140-3 Level 3, and Cosmos DB and Azure Storage support it for customer-managed keys. **Purge protection** means deleted keys stay recoverable during the retention period, and no one can purge them.
>
> - *A:* uses software-protected keys in a multi-tenant service.
> - *B:* has HSM-backed keys, but the service is **multi-tenant**.
> - *C:* single-tenant, but customer-managed rather than fully managed, and not natively integrated with PaaS customer-managed keys.
---

### Question 17 — Drag and Drop

You need to place subscriptions in the management group hierarchy according to the Azure landing zone architecture.

To answer, drag the appropriate management group to each subscription. Each management group may be used once, more than once, or not at all.

**Management groups:** Platform › Connectivity · Platform › Management · Platform › Identity · Landing Zones › Corp · Landing Zones › Online · Sandbox · Decommissioned

**Answer Area**

| Subscription | Management group |
|---|---|
| Subscription with the hub VNet, Azure Firewall, and ExpressRoute gateway | ? |
| Subscription with the central Log Analytics workspace | ? |
| Project Sentinel production (internet-facing POS API) | ? |
| Developer experimentation subscription with no corporate network connectivity | ? |

> [!faq]- Show Answer & Explanation
> | Subscription | Management group |
> |---|---|
> | Hub VNet / Firewall / ExpressRoute | **Platform › Connectivity** |
> | Central Log Analytics | **Platform › Management** |
> | Project Sentinel production | **Landing Zones › Online** |
> | Developer experimentation | **Sandbox** |
>
> **Online** is for internet-facing workloads, and **Corp** is for internal workloads connected to the corporate network. Sandbox subscriptions get relaxed policies and no hybrid connectivity. Decommissioned holds subscriptions that are being cancelled.
---

### Solution Series — Questions 18 to 20

> [!note] Shared scenario for Questions 18–20
> You need to recommend a solution that meets the diagnostic logging requirement in the security and compliance requirements.
>
> Each question presents a unique solution. Determine whether the solution meets the goal. **After you answer a question in this section, you will NOT be able to return to it.**

### Question 18

**Solution:** You assign an Azure Policy definition that uses the `Deny` effect to block resources that do not have diagnostic settings. You assign the policy to the Landing Zones management group.

Does this meet the goal?

A) Yes  
B) No  

> [!faq]- Show Answer & Explanation
> **Answer: B (No)** — `Deny` blocks the deployment, which violates "deployments that omit diagnostic settings must NOT fail." Diagnostic settings are also a separate child resource (`Microsoft.Insights/diagnosticSettings`), so a `Deny` on the parent resource can't evaluate them reliably. It also doesn't remediate existing resources.
---

### Question 19

**Solution:** You assign the built-in policy initiative that deploys diagnostic settings to a Log Analytics workspace by using the `DeployIfNotExists` effect. You assign the initiative to the Landing Zones management group, grant its managed identity the Log Analytics Contributor and Monitoring Contributor roles, and create a remediation task.

Does this meet the goal?

A) Yes  
B) No  

> [!faq]- Show Answer & Explanation
> **Answer: A (Yes)** — `DeployIfNotExists` lets the resource deployment succeed, then deploys the diagnostic settings automatically. Assigning at the management group covers every current and future landing zone subscription. The **remediation task** fixes **existing** resources, because DINE affects only new or updated resources by default. The managed identity needs rights to write the diagnostic settings and to the workspace.
---

### Question 20

**Solution:** You create an Azure Automation runbook that runs daily, queries Azure Resource Graph for resources without diagnostic settings, and configures the settings.

Does this meet the goal?

A) Yes  
B) No  

> [!faq]- Show Answer & Explanation
> **Answer: B (No)** — The requirement is that diagnostic settings are configured "**automatically when resources are created**." A daily runbook leaves resources unmonitored for up to 24 hours and adds operational overhead. Azure Policy is the native governance control for this.
---

### Question 21 — Hot Area

You need to recommend a cost governance solution that meets the tagging and budget requirements.

**Answer Area**

| | Options |
|---|---|
| **Tag inheritance policy effect:** | Audit · Deny · Append · Modify |
| **Budget notification:** | Azure Advisor cost recommendations · A Cost Management budget on each subscription with an 80% actual-cost alert and an action group · An Azure Monitor metric alert · Cost anomaly alerts |

> [!faq]- Show Answer & Explanation
> **Box 1: Modify** · **Box 2: A Cost Management budget with an 80% alert**
>
> The built-in *"Inherit a tag from the resource group if missing"* policy uses **Modify**, which adds or replaces tags during the request without blocking it. It also supports remediation tasks for existing resources. Append is legacy and can't remediate. Deny blocks deployments, and Audit only reports.
>
> Budgets send threshold alerts (actual or forecasted) to emails or action groups. Anomaly alerts flag unusual spend, not budget thresholds.
---

### Question 22

You need to recommend a solution that meets the distributed tracing requirement for the microservices.

What should you recommend?

A) Send Container Apps console logs to Log Analytics.  
B) Instrument the microservices with OpenTelemetry by using the Azure Monitor OpenTelemetry Distro, and send the telemetry to a workspace-based Application Insights resource.  
C) Configure Network Watcher connection monitor between the container apps.  
D) Enable VM insights on the Container Apps environment.  

> [!faq]- Show Answer & Explanation
> **Answer: B** — OpenTelemetry is the vendor-neutral standard. Application Insights correlates traces across services by using W3C trace context and provides the application map and end-to-end transaction views.
>
> - *A:* console logs have no trace correlation.
> - *C:* monitors network reachability, not application traces.
> - *D:* VM insights doesn't apply to serverless Container Apps.
---

## 📊 Quick Answer Summary

| Q | Type | Answer | Topic |
|:---:|---|---|---|
| 1 | MC | **D** | Session consistency (read-your-writes + multi-region writes) |
| 2 | Hot Area | Premium · Kafka endpoint (config only) | Event Hubs tier sizing & Kafka compatibility |
| 3 | MC | **C** | Event Hubs Capture (no-code archive) |
| 4 | MC | **D** | Service Bus sessions + duplicate detection |
| 5 | MC | **C** | Hierarchical partition key (20 GB logical partition limit) |
| 6 | MC | **A** | Custom conflict resolution (field-level merge) |
| 7 | MC | **D** | Fabric Mirroring for Cosmos DB (zero-ETL, no RU impact) |
| 8 | MC | **C** | Continuous backup 30-day PITR |
| 9 | Hot Area | User-assigned MI · Built-in Data Contributor | Cosmos DB data plane RBAC, keys disabled |
| 10 | Hot Area | Autoscale · Cosmos DB reserved capacity | Throughput & commitment cost optimization |
| 11 | Multi-select | **A, C, D** | Private, keyless Azure OpenAI |
| 12 | Hot Area | KEDA `azure-eventhub` · 2 | Event-lag scaling in Container Apps |
| 13 | Hot Area | App Gateway WAF_v2 mTLS · APIM internal `rate-limit-by-key` | Secure POS API ingress |
| 14 | Hot Area | Firewall Premium · UDR `0.0.0.0/0` | Forced egress with TLS inspection & IDPS |
| 15 | MC | **C** | Central Private DNS + DNS Private Resolver |
| 16 | MC | **D** | Managed HSM + purge protection |
| 17 | Drag & Drop | Connectivity · Management · Online · Sandbox | Landing zone management group placement |
| 18 | Solution | **B (No)** | Deny policy for diagnostics |
| 19 | Solution | **A (Yes)** | DeployIfNotExists + remediation task |
| 20 | Solution | **B (No)** | Daily Automation runbook |
| 21 | Hot Area | Modify · Budget 80% alert | Tag inheritance & budget alerts |
| 22 | MC | **B** | OpenTelemetry + Application Insights |

---

## 🔗 Related Vault References
- [AZ-305 Master Index](../README.md)
- [AZ-305 Practice Quiz & Mock Tracker](az-305-quiz.md)
- [Case Study 1: Contoso Pharmaceuticals](az-305-case-study-1.md)
- [NoSQL & Azure Cosmos DB](../Storage4%20-%20non%20relational%20database%20%28nosql%29.md)
- [Governance & Azure Policy](../Governance%20-%20Azure%20Policy.md)
- [Network Security & Isolation](../Networking%20-%20Security.md)
