---
tags:
  - studynote
  - az-305
title: Storage - non relational database (nosql)
author: Pralaya Panta
---


```table-of-contents
```

## Definition
- Non-relational databases
	- trades off consistency for scalability and availability
	- stores non-tabular data

## Data Types & Solutions
- key/value
	- stored in keys to identify the entity and value that contains the data
	- solutions
		- azure table storage
		- azure cosmos db
- document
	- collection of semi structure doc with fields & data identified using unique keys
	- solutions
		- azure cosmos db
- graph
	- stores nodes and edges
	- solution
		- azure cosmos db
		- azure SQL



## Table Storage vs. Azure Cosmos DB for Table
- **Azure Table Storage:**
	- low-cost regional key-value storage (`LRS / ZRS / GRS / RA-GRS`), up to **20,000 operations/sec**
	- single write region with one optional readable secondary region (`RA-GRS`) and manual failover
	- only primary index on `PartitionKey` and `RowKey` (no secondary indexes)
- **Azure Cosmos DB for Table:**
	- global turn-key multi-region distribution, automatic indexing on **all properties** (secondary indexes), `<10ms` read/write latency SLA, automatic failover, and up to millions of operations/sec
	- applications written for Azure Table Storage can migrate to Azure Cosmos DB for Table with minimal or zero code changes


## Azure Cosmos DB
- fully managed NoSQL and distributed database service for modern app development
	- takes database administration off your hands with automatic management, updates, and patching
	- handles capacity management with cost-effective serverless and automatic scaling options
- has single-digit millisecond response times (`<10ms` at the 99th percentile) and guaranteed speed at any scale

### The 6 Supported Cosmos DB APIs
- Azure Cosmos DB for NoSQL (Core / Native API):
	- stores JSON documents queried via SQL syntax; 
	- default recommendation for all new greenfield NoSQL workloads
- Azure Cosmos DB for MongoDB:
	- wire-protocol compatibility for existing MongoDB applications; offers two architectures:
		- Request Unit (RU) architecture: serverless or provisioned RU model with instant elasticity
		- vCore architecture: dedicated CPU/memory/disk cluster nodes for lift-and-shift MongoDB enterprise workloads and integrated vector search
- Azure Cosmos DB for Apache Cassandra:
	- wide-column store compatible with Apache Cassandra (CQL v4) for migrating Cassandra clusters
- Azure Cosmos DB for Apache Gremlin:
	- graph database storing vertices (entities) and edges (relationships) for social networks, fraud detection, and recommendation engines
- Azure Cosmos DB for Table:
	- key-value store compatible with Azure Table Storage SDKs, adding global distribution, secondary indexes, and `99.999%` SLA
- Azure Cosmos DB for PostgreSQL:
	- managed distributed relational PostgreSQL powered by the Citus extension for horizontal sharding across worker nodes

### The 5 Consistency Levels (Strongest to Weakest)
- Strong:
	- linearizability guarantee — reads are guaranteed to return the most recent committed write globally
	- Trade-off: highest latency (`2x` RU cost for reads compared to Session/Eventual); cannot be configured on accounts with more than one write region (Multi-Region Writes)
- Bounded Staleness:
	- reads lag behind writes by at most `K` versions (updates) OR `T` time interval (whichever is reached first; minimum `K=10` / `T=5s` for single region, `K=100,000` / `T=300s` for multi-region)
	- guarantees global total order outside the staleness window; commonly chosen by globally distributed apps needing Strong-like ordering with lower write latency
- Session (Default — Most Common):
	- scoped to a single client session token; guarantees Read-Your-Own-Writes, Monotonic Reads, Monotonic Writes, and Writes-Follow-Reads within the session
	- half the read RU cost of Strong / Bounded Staleness
- Consistent Prefix:
	- guarantees reads never see out-of-order writes (if writes were `A, B, C`, a reader sees `A`, `A, B`, or `A, B, C` — never `A, C`)
- Eventual:
	- weakest consistency, lowest latency — no ordering guarantee for reads, replicas converge eventually in the absence of new writes

### Partitioning & Partition Key Design
- **Logical Partition:**
	- groups items that share the exact same **Partition Key** value
	- **maximum size is 20 GB** per logical partition
- **Physical Partition:**
	- internal storage and compute unit managed automatically by Cosmos DB (supports up to **50 GB** storage and **10,000 RU/s** per physical partition)
- **Choosing an Effective Partition Key:**
	- select a **high-cardinality** property (thousands of distinct values such as `UserId`, `DeviceId`, `OrderId`, `TenantId`) that distributes both storage volume and RU/s evenly across partitions to prevent **Hot Partitions (`HTTP 429 TooManyRequests`)**
	- **Hierarchical Partition Keys (Subpartitioning):** chains up to **3 levels of keys** (e.g., `TenantId -> UserId -> SessionId`) so single tenants can scale beyond the 20 GB logical partition limit while still enabling targeted cross-subpartition queries
	- **Synthetic Partition Key:** concatenates multiple properties (e.g., `DeviceId_Date`) or appends a random number suffix (`1..N`) to spread write-heavy workloads

### Throughput & Capacity Modes
- **Provisioned Throughput (Standard):**
	- manually reserve a fixed number of Request Units per second (`RU/s`, minimum `400 RU/s`) at the Container or Database level for predictable, steady traffic (supports 1-year or 3-year Reserved Capacity discounts)
- **Provisioned Throughput (Autoscale):**
	- automatically scales instantaneously between **`10%` and `100%`** of a configured maximum `RU/s` (`Max RU/s`) based on real-time load with zero throttling during traffic spikes
- **Serverless:**
	- pay strictly per Request Unit consumed + storage used with **no minimum RU reservation** (max `5,000 RU/s` burst per container, max `1 TB` storage; ideal for spiky, intermittent, new, or dev/test workloads)

### High Availability, Multi-Region Writes & Core Features
- **Availability SLAs:**
	- **Single-Region Account:** `99.99%` availability (enable **Availability Zones** for zone fault tolerance)
	- **Multi-Region Account (Single-Region Writes + 1 or more Read Regions):** `99.99%` write SLA, **`99.999%` read SLA**
	- **Multi-Region Writes (Active-Active across regions):** **`99.999%` ("Five Nines") SLA for both reads and writes**
- **Conflict Resolution in Multi-Region Writes:**
	- **Last Writer Wins (Default):** uses the system `_ts` timestamp (or a custom user-defined numeric property) to resolve concurrent write conflicts automatically
	- **Custom Conflict Resolution:** executes a developer-defined **Merge Stored Procedure** (JavaScript) or writes conflicts to the conflict feed for asynchronous resolution
- **Change Feed:**
	- persistent, ordered log of all inserts and updates in a container in the order they were modified
	- used to trigger **Azure Functions**, **Azure Event Hubs**, or build real-time materialized views in microservices
- **Time to Live (TTL):**
	- automatically deletes expired items from a container after `N` seconds (configurable at the container level or per item) using background leftover RUs without impacting application throughput


https://learn.microsoft.com/en-us/azure/cosmos-db/overview
https://learn.microsoft.com/en-us/azure/cosmos-db/consistency-levels
https://learn.microsoft.com/en-us/azure/cosmos-db/partitioning-overview
