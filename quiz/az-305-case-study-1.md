---
title: "AZ-305 Case Study 1 — Contoso Pharmaceuticals"
tags: [mscerts, az-305, case-study, azure, architecture, hybrid, sql, aks, bcdr, vwan]
created: 2026-10-09
updated: 2026-10-10
type: case-study
exam: AZ-305
exam-date: 2026-10-16
total-questions: 22
---

# AZ-305 Case Study 1 — Contoso Pharmaceuticals

```
📅 AZ-305 Exam Date: October 16, 2026 — 🔴 6 days away
Format: Full case study · 22 questions · Hot Area, Solution Series, Drag & Drop, Multiple Choice
```

> [!warning] AI-Generated Study Material
> Synthetic practice scenario modelled on AZ-305 objectives. Not an official Microsoft exam item. Verify service limits against [Microsoft Learn](https://learn.microsoft.com/en-us/credentials/certifications/exams/az-305/).

> [!info] Case Study Instructions
> This is a case study. Case studies are not timed separately; you can use as much exam time as you would like to complete each case. To answer the questions, you must reference information provided in the case study. Case studies might contain exhibits and other resources that provide more information about the scenario. **Answer every question based only on the information in this case study.**
>
> Some questions in the **Solution Series** present a unique solution that might meet the stated goals. After you answer a question in that section, you will NOT be able to return to it.

---

## 📑 Case Study Dossier

### Overview

Contoso Pharmaceuticals is a biotechnology company headquartered in Basel. The company has a research center in New Jersey and branch offices in Amsterdam and Sydney. Contoso has approximately 8,500 users.

Contoso runs a Clinical Trial Management Platform (CTMP) that is used 24 hours a day by employees, partner hospitals, and regulatory auditors worldwide.

---

### Existing Environment

#### Active Directory

- The network contains two Active Directory Domain Services (AD DS) forests named `corp.contoso.com` and `trials.contoso.com`.
- `corp.contoso.com` contains all employee accounts. `trials.contoso.com` contains approximately 1,200 accounts for clinical investigators at partner hospitals.
- No trust relationship exists between the forests. The security team prohibits creating one.
- Contoso has a Microsoft Entra tenant named `contoso.com`. A server in Basel runs Microsoft Entra Connect Sync and synchronizes `corp.contoso.com` only.
- All users are licensed for Microsoft Entra ID P2.
- IT administrators have permanent Owner role assignments on the existing Azure pilot subscription.

#### Applications

- **CTMP** consists of a web front end and a REST API built on ASP.NET Core 8. Both tiers run on 40 virtual machines in Basel. The development team has containerized both tiers and uses Kubernetes in the test environment.
- **TrialScheduler** is a three-tier legacy application (database, application, and web tiers) that runs on 12 VMware vSphere virtual machines running Windows Server 2019. The application tier stores state on local disks and cannot be refactored or containerized. The tiers must start in this order: database, application, web.
- **Genomic Pipeline** runs nightly on an on-premises 64-node HPC cluster that uses InfiniBand interconnects and the Slurm workload manager. Jobs use Message Passing Interface (MPI), start at 22:00, run for approximately five hours, and must finish by 06:00. The research team maintains several hundred Slurm job scripts.

#### Databases

| Database | Platform | Size | Notes |
|---|---|---|---|
| CTMPDB | SQL Server 2019 Enterprise | 18 TB | Grows ~35% annually. Uses only database-scoped features. Reporting queries from the Clinical Analytics team cause blocking during business hours. Contains a `Patient` table with `NationalID` and `PatientName` columns. |
| LabInventoryDB | SQL Server 2016 Standard | 300 GB | Uses SQL Server Agent jobs, cross-database queries to a database named LabArchiveDB on the same instance, and Service Broker. |

#### File Storage

- Laboratory instruments in Basel and New Jersey write DICOM images to SMB shares on local Windows Server 2019 file servers. Each site generates approximately 2 TB of new images per month. Researchers mostly access images that are less than 30 days old.
- A NAS appliance in Basel stores 450 TB of scanned, signed patient consent forms (PDF). Consent forms are rarely accessed after 90 days.

#### Network

- An MPLS network connects the Basel and New Jersey datacenters.
- The Amsterdam and Sydney offices connect to the internet through local ISPs and use third-party firewall appliances.

#### Monitoring and Licensing

- System Center Operations Manager (SCOM) monitors all Windows servers. The operations team reviews System event log warnings daily.
- Contoso has Windows Server Datacenter licenses with active Software Assurance.

---

### Requirements

#### Planned Changes

- Migrate all workloads to Azure. The primary region will be **West Europe**. The secondary region will be **North Europe**.
- Retire the MPLS network. The Basel and New Jersey datacenters will remain operational for laboratory instruments and will connect to Azure by using ExpressRoute. The Amsterdam and Sydney offices will connect by using site-to-site VPN.
- Deploy CTMP to Azure Kubernetes Service (AKS) clusters in both regions.
- Migrate the TrialScheduler virtual machines to Azure without modifying the application.
- Decommission SCOM.

#### Business Requirements

- Minimize administrative effort. Use platform as a service (PaaS) where possible.
- Minimize costs, provided all other requirements are met.
- Use existing licensing investments where possible.

#### CTMP Requirements

- Users worldwide must connect by using a single global host name. TLS must terminate at the network edge location closest to each user.
- If West Europe becomes unavailable, user traffic must fail over to North Europe automatically, without waiting for DNS records to expire.
- AKS nodes and ingress controllers must NOT have public IP addresses.
- All web requests must be inspected by a web application firewall (WAF) that uses managed rule sets before the requests reach an Azure region.
- AKS workloads must access Azure Key Vault without storing credentials.
- Container images must be pulled privately from both regions. Image pulls must remain available if the West Europe registry endpoint is unavailable.

#### Database Requirements

- CTMPDB must be able to grow to at least 25 TB without downtime for storage scaling.
- Reporting queries must run on separate compute that can be sized independently of the primary database.
- In a regional disaster, CTMPDB must have an RPO of less than one minute and an RTO of less than one hour. Application connection strings must NOT change after a failover.
- LabInventoryDB must be migrated with minimal code changes.

#### TrialScheduler Requirements

- In a regional outage, TrialScheduler must be recovered in North Europe with an RPO of 15 minutes and an RTO of two hours. The tier start order must be preserved.
- Daily backups must be retained for one year and must be restorable in North Europe if West Europe is unavailable.
- The virtual machine sizes and region will not change for at least three years.

#### Genomic Pipeline Requirements

- Existing Slurm job scripts must be reused without modification.
- Jobs require a low-latency interconnect for MPI.
- No compute charges must be incurred when no jobs are running.

#### File Requirements

- Laboratory instruments must continue to write to local SMB shares.
- Images less than 30 days old must be cached locally. All images must be stored centrally in Azure.
- Minimize on-premises storage capacity.

#### Network Requirements

- All traffic between offices, datacenters, Azure virtual networks, and the internet must be inspected by a centrally managed firewall service in each Azure region.
- Minimize the management of user-defined routes (UDRs).
- Datacenter routers must prefer ExpressRoute for traffic to Azure. Each datacenter must also have a VPN connection to use if ExpressRoute fails.

#### Monitoring Requirements

- Collect Warning and Error events from the System log of all Windows virtual machines in Azure to a central location that supports queries and alerts.
- Collect only those events to minimize costs.

#### Security and Compliance Requirements

- Consent forms must be stored in a write once, read many (WORM) state for 10 years. No user, including administrators, must be able to modify or delete them during that period.
- Auditors must be able to open any consent form within one hour of a request. Minimize storage costs.
- The `NationalID` and `PatientName` columns must be encrypted so that database administrators and Azure administrators cannot view plaintext values. The application must support equality lookups on `NationalID` and `LIKE` pattern searches on `PatientName`.
- Users in `trials.contoso.com` must authenticate to CTMP by using their on-premises credentials. Minimize the number of on-premises servers.
- Administrative access to Azure resources must NOT be standing. Activation must require MFA and approval and must expire after a maximum of four hours.
- Destructive backup operations must require approval from the security team.
- Auditors require a catalog of all data assets in Azure SQL and Azure Blob Storage, including automated classification and data lineage.

---

## ❓ Questions

### Question 1 — Hot Area

You need to recommend a solution for CTMPDB that meets the database requirements and the business requirements.

What should you include in the recommendation? To answer, select the appropriate options in the answer area.

**NOTE:** Each correct selection is worth one point.

**Answer Area**

| | Options |
|---|---|
| **Database service:** | Azure SQL Database Business Critical · Azure SQL Database Hyperscale · Azure SQL Managed Instance Business Critical · SQL Server on Azure Virtual Machines |
| **Reporting workload:** | A high-availability secondary replica accessed with `ApplicationIntent=ReadOnly` · A named replica · A geo-replica · Elastic query |

> [!faq]- Show Answer & Explanation
> **Box 1: Azure SQL Database Hyperscale** — Hyperscale scales storage to 100 TB+ without downtime. Azure SQL Database Business Critical caps at 4 TB, and SQL Managed Instance Business Critical caps at 16 TB, so neither can reach the 25 TB requirement. SQL Server on VMs works but violates "use PaaS / minimize administrative effort". CTMPDB uses only database-scoped features, so Azure SQL Database is compatible.
>
> **Box 2: A named replica** — The key phrase is "compute that can be **sized independently** of the primary". Hyperscale HA secondary replicas must use the same service level objective as the primary. Named replicas can have a different compute size and are isolated from the primary's HA replicas. A geo-replica is for DR, not for sizing reporting compute. Elastic query is for cross-database querying.
---

### Question 2 — Hot Area

You need to recommend a disaster recovery solution for CTMPDB.

What should you include? To answer, select the appropriate options in the answer area.

**Answer Area**

| | Options |
|---|---|
| **DR feature:** | Active geo-replication only · An auto-failover group · Zone redundancy · Geo-restore from geo-redundant backups |
| **Application connection target:** | The primary logical server name · The failover group read-write listener endpoint · The named replica endpoint · The private endpoint IP address |

> [!faq]- Show Answer & Explanation
> **Box 1: An auto-failover group** · **Box 2: The failover group read-write listener endpoint**
>
> Failover groups (supported for Hyperscale) replicate asynchronously with an RPO measured in seconds. They provide a listener DNS name (`<fog-name>.database.windows.net`) that always points to the current primary, so **connection strings don't change** after a failover.
>
> - *Active geo-replication only:* gives the same RPO, but the app must be repointed to the secondary server name after failover.
> - *Zone redundancy:* protects against a datacenter failure, not a regional disaster.
> - *Geo-restore:* RPO is up to 1 hour and RTO is many hours for an 18 TB database, so it fails both targets.
---

### Question 3

You need to recommend a target platform for LabInventoryDB.

What should you recommend?

A) Azure SQL Database Hyperscale  
B) Azure SQL Database in an elastic pool  
C) Azure SQL Managed Instance  
D) SQL Server on Azure Virtual Machines  

> [!faq]- Show Answer & Explanation
> **Answer: C** — SQL Managed Instance gives near-100% compatibility with instance-scoped features: **SQL Server Agent**, **cross-database queries** within the instance, and **Service Broker** within the instance. Migrate LabArchiveDB to the same instance and no code changes are needed.
>
> - *A and B:* Azure SQL Database doesn't support SQL Agent, Service Broker, or three-part-name cross-database queries.
> - *D:* SQL on VMs works, but it's IaaS and violates "minimize administrative effort / prefer PaaS".
---

### Solution Series — Questions 4 to 6

> [!note] Shared scenario for Questions 4–6
> You need to recommend an ingress solution for CTMP that meets the CTMP requirements.
>
> Each question presents a unique solution. Determine whether the solution meets the goal. **After you answer a question in this section, you will NOT be able to return to it.**

### Question 4

**Solution:** You deploy Azure Application Gateway v2 with WAF in West Europe and North Europe. Each Application Gateway routes to an internal load balancer in front of the AKS ingress controller. You deploy Azure Traffic Manager with priority routing in front of both Application Gateways.

Does this meet the goal?

A) Yes  
B) No  

> [!faq]- Show Answer & Explanation
> **Answer: B (No)** — Traffic Manager is DNS-based. It doesn't terminate TLS at a global edge (TLS terminates in the Azure region), and failover depends on clients' DNS TTL expiring. That fails two explicit requirements: "TLS must terminate at the network edge closest to each user" and "without waiting for DNS records to expire". WAF inspection also happens inside the region, not *before* traffic reaches the region.
---

### Question 5

**Solution:** You deploy Azure Front Door Standard with a WAF policy. The origins are the public Standard load balancers of the AKS clusters. You configure network security groups (NSGs) to allow inbound traffic only from the `AzureFrontDoor.Backend` service tag.

Does this meet the goal?

A) Yes  
B) No  

> [!faq]- Show Answer & Explanation
> **Answer: B (No)** — Two failures:
> 1. The AKS ingress has a **public IP address**, which is explicitly prohibited. Restricting it with NSGs doesn't remove the public IP.
> 2. **Front Door Standard supports custom WAF rules only.** Managed rule sets (DRS/OWASP) and Private Link origins require **Front Door Premium**.
---

### Question 6

**Solution:** You deploy Azure Front Door Premium with a WAF policy that uses the Default Rule Set. In each region, you expose the AKS internal load balancer through an Azure Private Link service and configure it as a Private Link origin in the same origin group.

Does this meet the goal?

A) Yes  
B) No  

> [!faq]- Show Answer & Explanation
> **Answer: A (Yes)** — Front Door Premium terminates TLS at 190+ edge PoPs, runs managed WAF rule sets at the edge, and fails over between origins based on health probes (no DNS TTL dependency). Private Link origins connect to the AKS **internal** load balancer over the Microsoft backbone, so no public IP is needed on the nodes or ingress.
---

### Question 7

You need to recommend a solution that allows the CTMP pods to access Azure Key Vault. The solution must meet the CTMP requirements.

What should you recommend?

A) Store a service principal client secret in a Kubernetes secret and mount it into each pod.  
B) Use the AKS kubelet managed identity for all pods.  
C) Enable Microsoft Entra pod-managed identity (preview) on the clusters.  
D) Enable Microsoft Entra Workload ID and federate a user-assigned managed identity with a Kubernetes service account.  

> [!faq]- Show Answer & Explanation
> **Answer: D** — Microsoft Entra Workload ID uses OIDC federation between the AKS cluster issuer and a user-assigned managed identity. Pods exchange a projected service account token for an Entra token, so **no credentials are stored**.
>
> - *A:* Stores a credential, which violates the requirement.
> - *B:* The kubelet identity is shared by every pod on the node, which breaks least privilege.
> - *C:* Pod-managed identity is deprecated and was replaced by Workload ID.
---

### Question 8 — Hot Area

You need to recommend a container registry configuration that meets the CTMP requirements.

**Answer Area**

| | Options |
|---|---|
| **Azure Container Registry SKU:** | Basic · Standard · Premium |
| **Feature:** | Zone redundancy only · Geo-replication to North Europe · Content trust · Import pipelines |

> [!faq]- Show Answer & Explanation
> **Box 1: Premium** · **Box 2: Geo-replication to North Europe**
>
> Only **Premium** supports geo-replication and private endpoints ("pulled privately"). With geo-replication, AKS in each region pulls from its local replica. If the West Europe endpoint is unavailable, North Europe pulls continue from the North Europe replica. Zone redundancy alone only protects within West Europe.
---

### Question 9

You need to recommend a compute solution for the Genomic Pipeline.

What should you recommend?

A) Azure Batch with a pool of dedicated HB-series nodes  
B) Azure CycleCloud with a Slurm cluster that uses autoscaling HBv4-series nodes  
C) An AKS cluster with an autoscaling GPU node pool  
D) A virtual machine scale set of Spot HB-series virtual machines  

> [!faq]- Show Answer & Explanation
> **Answer: B** — The deciding requirement is "**Slurm job scripts must be reused without modification**." Azure CycleCloud deploys and manages Slurm clusters natively, autoscales nodes on queue demand, and scales to zero when idle. HB-series VMs provide InfiniBand for MPI.
>
> - *A:* Azure Batch is an excellent HPC PaaS, but jobs must be resubmitted through the Batch API/task model. Slurm scripts can't be reused as-is.
> - *C:* AKS isn't designed for tightly coupled MPI over InfiniBand.
> - *D:* Spot evictions risk missing the 06:00 deadline, and a VMSS provides no Slurm scheduler.
---

### Question 10

You need to recommend a solution for the laboratory DICOM images that meets the file requirements.

What should you recommend?

A) Azure File Sync with cloud tiering enabled on the local file servers, using a date policy of 30 days  
B) Azure Data Box Gateway deployed at each site  
C) Azure NetApp Files volumes mounted over ExpressRoute  
D) A scheduled AzCopy task that moves images to Azure Blob Storage nightly  

> [!faq]- Show Answer & Explanation
> **Answer: A** — Azure File Sync keeps the local SMB shares (instruments keep writing locally) and syncs everything to a central Azure file share. Cloud tiering with a **30-day date policy** keeps recent images cached locally and replaces older files with pointers, which minimizes on-premises capacity.
>
> - *B:* Data Box Gateway is for one-way transfer, not a multi-site cache.
> - *C:* Azure NetApp Files has no local cache, so instruments would write over the WAN.
> - *D:* AzCopy moves files but doesn't provide transparent local access to tiered files.
---

### Question 11 — Hot Area

You need to recommend a storage configuration for the consent forms that meets the security and compliance requirements.

**Answer Area**

| | Options |
|---|---|
| **Access tier after 90 days (via lifecycle management):** | Hot · Cool · Cold · Archive |
| **Data protection:** | A time-based retention policy in the unlocked state · A time-based retention policy in the locked state · A legal hold · Blob soft delete with 3,650-day retention |

> [!faq]- Show Answer & Explanation
> **Box 1: Cold** — The trap is **Archive**. It's the cheapest tier, but it's offline. Standard rehydration takes up to 15 hours, and high-priority rehydration is not guaranteed under one hour. That fails "open any consent form within one hour". **Cold** is online (millisecond access), cheaper than Cool, and its 90-day minimum retention fits the access pattern.
>
> **Box 2: Time-based retention policy, locked** — A locked policy can't be removed or shortened by anyone, including Owners and Microsoft Support, so it meets SEC 17a-4(f)/FDA-style WORM requirements. An unlocked policy can be deleted, a legal hold can be cleared by authorized users, and soft delete isn't WORM. Tier changes are still allowed on immutable blobs.
---

### Question 12 — Hot Area

You need to recommend a disaster recovery solution for TrialScheduler.

**Answer Area**

| | Options |
|---|---|
| **Replication service:** | Azure Backup with Cross Region Restore · Azure Site Recovery · Availability zones · Azure Migrate |
| **Preserve tier start order by using:** | A recovery plan with multiple groups · An availability set · Load balancer health probes · VM boot diagnostics |

> [!faq]- Show Answer & Explanation
> **Box 1: Azure Site Recovery** · **Box 2: A recovery plan with multiple groups**
>
> Site Recovery replicates Azure VMs continuously to North Europe, with RPOs measured in seconds to minutes (well under 15 minutes). A **recovery plan** puts the database, application, and web tiers into ordered groups (Group 1 → 2 → 3) and can run Automation runbooks between groups, recovering within the 2-hour RTO.
>
> Azure Backup restores can't meet a 15-minute RPO. Availability zones don't cover a regional outage.
---

### Question 13 — Hot Area

You need to recommend a backup configuration for the TrialScheduler virtual machines.

**Answer Area**

| | Options |
|---|---|
| **Recovery Services vault storage redundancy:** | LRS · ZRS · GRS with Cross Region Restore enabled |
| **Protect destructive operations by using:** | Soft delete · Multi-user authorization (MUA) with a Resource Guard in a subscription owned by the security team · The Backup Contributor role · A CanNotDelete resource lock |

> [!faq]- Show Answer & Explanation
> **Box 1: GRS with Cross Region Restore** · **Box 2: MUA with a Resource Guard**
>
> GRS replicates backup data to the **paired region**. West Europe pairs with North Europe, which matches the requirement. CRR lets you restore in North Europe at any time, without waiting for Microsoft to declare a regional disaster.
>
> MUA requires the operator to get Contributor access on a Resource Guard that the **security team** controls before critical operations (disabling soft delete, reducing retention, stopping protection with data deletion). Soft delete alone doesn't require approval.
>
> > [!tip] Exam trap
> > GRS always replicates to the *paired* region. If a question pairs West Europe with East US, CRR can't restore there.
---

### Question 14 — Drag and Drop

You need to migrate the TrialScheduler virtual machines from VMware to Azure. The solution must minimize downtime, validate the migrated VMs before cutover, and avoid installing agents on the VMs.

Which five actions should you perform in sequence? To answer, move the appropriate actions from the list of actions to the answer area and arrange them in the correct order.

**Actions**
- Create a Recovery Services vault.
- Run a test migration.
- Create an Azure Migrate project.
- Install the Mobility service on each VM.
- Start replication.
- Deploy and configure the Azure Migrate appliance in vCenter Server.
- Migrate the VMs.

> [!faq]- Show Answer & Explanation
> **Correct sequence:**
> 1. Create an Azure Migrate project.
> 2. Deploy and configure the Azure Migrate appliance in vCenter Server.
> 3. Start replication.
> 4. Run a test migration.
> 5. Migrate the VMs.
>
> The Azure Migrate **agentless** VMware migration uses the appliance (snapshot-based replication through vCenter APIs). The *Mobility service* is only for agent-based migration, which the requirements rule out. A *Recovery Services vault* is for Site Recovery/Backup, not Azure Migrate. The test migration validates the VMs in an isolated VNet before cutover, which minimizes risk and downtime.
---

### Question 15 — Hot Area

You need to recommend a network design that meets the network requirements.

**Answer Area**

| | Options |
|---|---|
| **Azure Virtual WAN SKU:** | Basic · Standard |
| **Traffic inspection:** | NSGs on every spoke subnet · A third-party NVA in each spoke VNet with UDRs · Azure Firewall in a single hub in West Europe · Azure Firewall in each virtual hub, with routing intent for private and internet traffic |

> [!faq]- Show Answer & Explanation
> **Box 1: Standard** · **Box 2: Azure Firewall in each virtual hub with routing intent**
>
> Basic Virtual WAN supports only site-to-site VPN. It doesn't support ExpressRoute, VNet-to-VNet transit, or secured hubs. Standard Virtual WAN with **secured virtual hubs** (Azure Firewall managed by Firewall Manager) inspects traffic in **each region**. **Routing intent** applies private and internet routing policies automatically, so no UDRs need to be managed. A single West Europe hub would hairpin North Europe traffic and isn't "in each Azure region".
---

### Question 16

You need to ensure that the Basel and New Jersey datacenter routers meet the network requirements for path selection.

What should you configure on the on-premises edge routers?

A) Advertise more specific prefixes over the VPN connection than over ExpressRoute.  
B) Add static routes to Azure over the VPN with a lower administrative distance.  
C) Assign a higher BGP local preference to routes learned from ExpressRoute.  
D) Disable BGP on the VPN connection and fail over manually.  

> [!faq]- Show Answer & Explanation
> **Answer: C** — For outbound path selection on the on-premises router, **local preference** is the BGP attribute that decides which learned route is preferred. A higher local preference on ExpressRoute-learned routes makes ExpressRoute the primary path. The VPN routes stay in the table and take over automatically if the ExpressRoute BGP session drops. For identical prefixes on the Azure side, Azure already prefers ExpressRoute over VPN for the return path.
>
> - *A:* This is a classic trap. The longest prefix match always wins, so the VPN would become the **primary** path.
> - *B and D:* Static routes and manual failover add effort and don't fail over automatically.
---

### Question 17

You need to recommend an identity solution for the `trials.contoso.com` users that meets the security and compliance requirements.

What should you recommend?

A) Deploy a second Microsoft Entra Connect Sync server in `trials.contoso.com` in active mode.  
B) Deploy Microsoft Entra Cloud Sync provisioning agents in `trials.contoso.com` and enable password hash synchronization.  
C) Deploy an AD FS farm and Web Application Proxy servers in `trials.contoso.com`.  
D) Invite the `trials.contoso.com` users as Microsoft Entra B2B guests.  

> [!faq]- Show Answer & Explanation
> **Answer: B** — Cloud Sync supports **disconnected forests** with no trust between them. It uses lightweight agents that connect outbound over 443, and the sync engine runs in the cloud. It can **coexist** with the existing Connect Sync server that syncs `corp.contoso.com`. Deploy 2–3 agents for high availability, which keeps the on-premises footprint small.
>
> - *A:* Only one active Connect Sync server per tenant is supported (the others must be in staging mode).
> - *C:* AD FS adds several servers.
> - *D:* B2B guests don't sign in with their on-premises `trials` credentials.
---

### Question 18 — Hot Area

You need to recommend a solution for administrative access to Azure resources that meets the security requirements.

**Answer Area**

| | Options |
|---|---|
| **Service:** | Conditional Access · Microsoft Entra Privileged Identity Management (PIM) · Access reviews · Entitlement management |
| **Role assignment type:** | Permanent active · Active, time-bound · Eligible |

> [!faq]- Show Answer & Explanation
> **Box 1: PIM** · **Box 2: Eligible**
>
> Eligible assignments mean no standing access. PIM role settings enforce **MFA on activation**, **require approval**, and set a **maximum activation duration of 4 hours**. The P2 licenses in the existing environment cover PIM.
>
> - *Active, time-bound:* grants access for a date range without activation, so it's still standing access during that window.
> - *Conditional Access:* controls sign-in, not role activation.
---

### Question 19 — Hot Area

You need to recommend a monitoring solution that meets the monitoring requirements.

**Answer Area**

| | Options |
|---|---|
| **Agent:** | Log Analytics agent (MMA) · Azure Monitor Agent · Dependency agent · Azure Diagnostics extension |
| **Collection configuration:** | Diagnostic settings · A data collection rule (DCR) with an XPath query · An Azure Policy guest configuration · An Azure Monitor workbook |
| **Destination:** | Azure Storage account · Azure Event Hubs · Log Analytics workspace |

> [!faq]- Show Answer & Explanation
> **Box 1: Azure Monitor Agent** · **Box 2: A DCR with an XPath query** · **Box 3: Log Analytics workspace**
>
> The legacy MMA agent was retired in August 2024. A DCR associated with the VMs can use the XPath filter `System!*[System[(Level=2 or Level=3)]]` (Error = 2, Warning = 3), so **only** those events are collected, which minimizes ingestion cost. Log Analytics provides KQL queries and log search alerts, which replaces SCOM. Diagnostic settings are for Azure resource/platform logs, not guest OS event logs.
---

### Question 20

You need to recommend an encryption solution for the `NationalID` and `PatientName` columns in CTMPDB.

What should you recommend?

A) Always Encrypted with secure enclaves (VBS), using enclave-enabled column encryption keys  
B) Always Encrypted with deterministic encryption on both columns  
C) Transparent data encryption (TDE) with a customer-managed key  
D) Dynamic data masking on both columns  

> [!faq]- Show Answer & Explanation
> **Answer: A** — Always Encrypted keeps plaintext out of reach of DBAs and Azure administrators, because the keys are held by the client and Key Vault. The deciding requirement is **`LIKE` pattern searches**:
> - *Deterministic encryption* (B) supports equality only. It works for `NationalID` but not `LIKE` on `PatientName`.
> - *Secure enclaves* let the database engine run pattern matching, range comparisons, and sorting on encrypted columns inside a protected memory enclave.
>
> TDE (C) protects data at rest only; DBAs still see plaintext. Masking (D) isn't encryption, and privileged users bypass it.
---

### Question 21

You need to recommend a solution that meets the auditors' data catalog requirements.

What should you recommend?

A) The SQL Data Discovery & Classification feature on each database  
B) Microsoft Defender CSPM sensitive data discovery  
C) Azure Resource Graph queries  
D) Microsoft Purview Data Map and Unified Catalog  

> [!faq]- Show Answer & Explanation
> **Answer: D** — Microsoft Purview scans Azure SQL and Blob/ADLS sources, applies automated classifications, builds a searchable catalog, and captures **lineage** (for example, from Data Factory pipelines).
>
> - *A:* Classifies a single database, with no catalog or lineage.
> - *B:* Finds sensitive data to support security posture, but it isn't a governance catalog with lineage.
> - *C:* Inventories Azure resources, not data assets.
---

### Question 22 — Hot Area

You need to minimize the compute costs of the TrialScheduler virtual machines in Azure.

**Answer Area**

| | Options |
|---|---|
| **Licensing:** | Pay-as-you-go Windows Server licensing · Azure Hybrid Benefit · Dev/Test pricing |
| **Commitment:** | Azure Spot Virtual Machines · A 1-year Azure savings plan for compute · 3-year Reserved Virtual Machine Instances |

> [!faq]- Show Answer & Explanation
> **Box 1: Azure Hybrid Benefit** · **Box 2: 3-year Reserved VM Instances**
>
> Contoso owns Windows Server Datacenter licenses with Software Assurance, so AHB removes the Windows license cost ("use existing licensing investments"). The VM sizes and region are **fixed for 3 years**, which gives the deepest discount from 3-year reservations. Savings plans are better when size or region will change.
>
> - *Spot:* Spot VMs can be evicted, which is unacceptable for a production stateful app.
> - *Dev/Test pricing:* only for non-production subscriptions.
---

## 📊 Quick Answer Summary

| Q | Type | Answer | Topic |
|:---:|---|---|---|
| 1 | Hot Area | Hyperscale · Named replica | SQL tier selection & independent reporting compute |
| 2 | Hot Area | Auto-failover group · Read-write listener | Regional DR without connection string changes |
| 3 | MC | **C** | SQL Managed Instance for Agent / Service Broker / cross-DB |
| 4 | Solution | **B (No)** | App Gateway + Traffic Manager (DNS failover, regional TLS) |
| 5 | Solution | **B (No)** | Front Door Standard + public AKS origin |
| 6 | Solution | **A (Yes)** | Front Door Premium + Private Link origin |
| 7 | MC | **D** | Microsoft Entra Workload ID for AKS |
| 8 | Hot Area | Premium · Geo-replication | ACR resilient private pulls |
| 9 | MC | **B** | Azure CycleCloud + Slurm + HBv4 |
| 10 | MC | **A** | Azure File Sync cloud tiering (30-day date policy) |
| 11 | Hot Area | Cold · Locked time-based retention | WORM + 1-hour retrieval (Archive trap) |
| 12 | Hot Area | Site Recovery · Recovery plan groups | Ordered multi-tier VM DR |
| 13 | Hot Area | GRS + CRR · MUA Resource Guard | Paired-region restore & backup tamper protection |
| 14 | Drag & Drop | Project → Appliance → Replicate → Test → Migrate | Azure Migrate agentless VMware |
| 15 | Hot Area | Standard · Firewall per hub + routing intent | Virtual WAN secured hubs |
| 16 | MC | **C** | BGP local preference for ExpressRoute primary |
| 17 | MC | **B** | Entra Cloud Sync for disconnected forest |
| 18 | Hot Area | PIM · Eligible | JIT admin access |
| 19 | Hot Area | AMA · DCR XPath · Log Analytics | Filtered Windows event collection |
| 20 | MC | **A** | Always Encrypted with secure enclaves (`LIKE`) |
| 21 | MC | **D** | Microsoft Purview catalog & lineage |
| 22 | Hot Area | AHB · 3-year RI | Fixed-size VM cost optimization |

---

## 🔗 Related Vault References
- [[mscerts/az-305/README|AZ-305 Master Index]]
- [[mscerts/az-305/quiz/az-305-quiz|AZ-305 Practice Quiz & Mock Tracker]]
- [[mscerts/az-305/quiz/az-305-case-study-2|Case Study 2: Litware Financial Services]]
- [[mscerts/az-305/Storage3 - relational database|Relational Databases & Azure SQL]]
- [[mscerts/az-305/Networking - Connectivity|Hybrid Networking & Virtual WAN]]
- [[mscerts/az-305/Business Cont - Backup & recovery|Backup & Disaster Recovery]]
- [[mscerts/az-305/Monitoring - Solutions|Monitoring Solutions (AMA & DCR)]]
- **Relevant Skills:** [[.agents/skills/exam-prep/SKILL|exam-prep skill]] · [[.agents/skills/azure-deploy/SKILL|azure-deploy skill]]
