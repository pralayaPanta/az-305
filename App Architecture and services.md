---
tags:
  - studynote
  - az-305
title: App Architecture and Services
author: Pralaya Panta
updated: 2026-10-23
---

```table-of-contents
```



## Messaging Architecture

- what is message
	- contains raw data produced by one component 
		- consumed by another component
	- contains data itself
- consider messages if
	- sender expectation is destination will process 
		- needs guaranteed comms

###  Queue Mechanisms
 - Azure service bus queues
		 - part of messaging infrastructure (enterprise message broker)
		 - use if queue size is under 80 GB and you need FIFO ordering, duplicate detection, transactions, or pub/sub topics
-  Azure queue storage
		 - storage infrastructure (simple REST queue)
		 - use if queue size needs to exceed 80 GB (up to 500 TB), messages are <= 64 KB, and simple at-least-once delivery is sufficient
		 - provides server-side audit trail of all queue transactions in Azure Storage Analytics

### Azure Service Bus
- fully managed enterprise message broker
- receives messages without polling (long-polling / push-style receive)
- FIFO delivery (via Message Sessions) and automatic duplicate detection
- transactional behaviour, atomicity, auto-forwarding, and Dead-Letter Queue (DLQ) for poison/unprocessed messages
- tiers
	- Standard tier - shared multi-tenant capacity, pay-per-operation, max 256 KB message size, Queues + Topics/Subscriptions
	- Premium tier - dedicated Messaging Units (predictable CPU/memory), max 100 MB message size, Availability Zones, Geo-Disaster Recovery / Geo-Replication, VNet Service Endpoints, Private Link, and Customer-Managed Keys (CMK)
- supports 
	- message queues and 
	- publish-subscribe topics.

#### Service Bus Topics (publish-subscribe topics)
- 1:N fan-out model where publishers send to a Topic and multiple independent Subscriptions receive filtered copies (SQL, Boolean, or Correlation filters)
- can have multiple destinations for a single message with queue-like behavior per subscription

#### Service Bus Queues (message queues)
- 1:1 point-to-point competing consumers model
- groups messages into transactions
- receives messages without polling
- queues can handle messages larger than 64 KB (up to 256 KB Standard / 100 MB Premium) and queue size up to 80 GB
- supports Peek-Lock (at-least-once processing) and Receive-and-Delete (at-most-once processing)

## Event Driven Application
- An event is a lightweight notification that indicates something occurred.
- An event can be sent to multiple receivers or to none.

### Azure Event Hub
- fully managed, big data streaming platform and event ingestion service (millions of events/sec; Apache Kafka compatible)
- can send and receive events in diff language
- events received are added to the end of its data stream
- architecture
	- Partitions - ordered commit log; events stay in the partition for the retention period (1-90 days) and can be replayed
	- Consumer Groups - independent views/offsets of the entire event stream so multiple downstream analytics apps can read the same stream concurrently at their own pace
	- Event Hubs Capture - automatically writes streaming data directly to Blob Storage or ADLS Gen2 in Avro or Parquet format with zero code
- pull model
	- holds each message in its cache and allows it to be read
	- when message is read it is not deleted
	- messages remain for other consumers
- tiers
	- basic (1 consumer group, no Capture)
	- standard (up to 20 consumer groups, Capture, Kafka endpoint)
	- premium / dedicated (dedicated capacity, Private Link, CMK, up to 90-day retention)
- use case
	- live dashboarding, 
	- supporting analytics pipelines like clickstreams, and 
	- detecting anomalies like fraud or outlier actions.
- doesn't handle messages that aren't processed as expected

### Azure Event Grid
- reactive event routing and automation for discrete state changes (e.g., blob created, resource modified)
- aggregates all events and provides routing from src to dst
- concepts
	- Event Sources -> Topics (System Topics, Custom Topics, Partner Topics, Event Domains) -> Event Subscriptions (with prefix/suffix/payload filters) -> Event Handlers
- distributes event from sources like Azure Blob storage
- distributed to handlers like Az function and webhooks 
	- event handler --> subscribes
- supports
	- push delivery (HTTP/S with 24-hour exponential retry + optional dead-lettering to Blob Storage)
	- pull delivery & MQTT v3.1.1/v5.0 via Event Grid Namespace Topics (for IoT and pull consumers)
- scenarios
	- automating workflows
	- event broadcasting
- serverless

[Compare messaging services](https://learn.microsoft.com/en-us/azure/service-bus-messaging/compare-messaging-services)
### Azure Stream Analytics
- real time data processing
- scenarios
	- monitoring IoT devices telemetry
	- business analytics


### Azure IoT Hub
- securely managing IoT devices (bidirectional device-to-cloud telemetry and cloud-to-device commands, device twins, DPS zero-touch provisioning)
- device management
- telemetry integration


## Azure API Management
- API Gateway - acts as a unified façade for all backend APIs (accepts calls, verifies keys/JWTs/certs, enforces quotas, caches responses, transforms payloads)
- Management Plane - configure APIs, Products (API bundles), Subscriptions, and XML policies
- Developer Portal - self-service portal for developers to discover APIs, view OpenAPI docs, test endpoints, and request keys
- common XML policies (applied Inbound, Backend, Outbound, or On-Error)
	- validate-jwt (verify Entra ID / OAuth 2.0 tokens at the gateway)
	- rate-limit-by-key and quota-by-key (throttling and DoS protection)
	- ip-filter, check-header, cors (access control)
	- cache-lookup and cache-store (response caching)
	- rewrite-uri, set-header, xml-to-json, mock-response (transformation and mocking)
- tiers
	- Consumption tier - serverless pay-per-call, scales to zero (no Developer Portal, no VNet)
	- Classic tiers (Developer, Basic, Standard, Premium) - Premium supports Multi-Region deployment, Availability Zones, and VNet injection
	- v2 tiers (Basic v2, Standard v2, Premium v2) - fast deployment, simplified VNet integration and Private Endpoints
- VNet & hybrid connectivity
	- External VNet mode - gateway is public while backend APIs reside privately inside the VNet
	- Internal VNet mode - gateway has only a private VNet IP (place Application Gateway WAF or Front Door Premium + Private Link in front for external access)
	- Self-Hosted Gateway - containerized gateway deployed on-premises or multi-cloud (Kubernetes/Docker) to route local hybrid API traffic with low latency while managed centrally from Azure


## Azure Cache for Redis

- Definition - a fully managed, in-memory cache that enables high-performance data storage, distributed user session state, and low-latency retrieval
- Key features
	- high throughput and sub-millisecond latency
	- flexible data structures (strings, hashes, lists, sets, sorted sets, pub/sub)
	- data persistence (RDB snapshots / AOF logs)
- Caching patterns
	- Cache-Aside (lazy loading) - app checks Redis first; on cache miss, queries DB, writes result to Redis with a TTL, and returns it
	- Write-Through / Write-Behind
	- Read-Through
- Tiers
	- Basic - single node, no SLA (dev/test only)
	- Standard - replicated 2-node Primary/Replica pair (99.9% SLA)
	- Premium - adds data persistence, Redis clustering (horizontal sharding up to 10 shards), VNet injection / Private Link, and Zone Redundancy
	- Enterprise & Enterprise Flash (Azure Managed Redis) - supports Active Geo-Replication (active-active multi-region writes up to 99.999% SLA), RediSearch/Vector Search, RedisJSON, and RedisTimeSeries
- Azure Managed Redis is current recommended caching service
- Azure Managed Redis can help improve performance in apps that interface with many database solutions, including Azure SQL Database, Azure Cosmos DB, and Azure Database for MySQL.



## Azure App Configuration

- centrally manage non-secret application settings and feature flags across microservices, App Service, Functions, and AKS
- offers flexible key representations and mappings, and configuration snapshots - immutable point-in-time captures of all settings used for deployment rollback and audit
- dedicated UI for feature flag management, and supports resource tagging with environment labels (Dev, Staging, Prod)
- Sentinel key pattern - updating a single watched sentinel key triggers dynamic runtime configuration refresh across running apps without restarting them
- Key Vault references - App Configuration stores non-secret settings and feature flags while referencing Azure Key Vault for sensitive secrets, keys, and certificates
- sensitive info can be encrypted at rest (supports CMK) and in transit
- geo-replication allows App Configuration stores to be replicated across Azure regions
- development
	- visual studio, code, Azure CLI
- production
	- azure, entra managed identities for azure



## Quick Link
[Azure Queue Storage](https://azure.microsoft.com/services/storage/queues/)
[Azure Service Bus](https://azure.microsoft.com/services/service-bus/)
[Azure Event Grid](https://azure.microsoft.com/services/event-grid/)
[Azure Resource Manager (ARM) templates](https://learn.microsoft.com/en-us/azure/azure-resource-manager/templates/overview)
[Azure Automation](https://learn.microsoft.com/en-us/azure/automation/automation-intro)
[Azure App Configuration](https://learn.microsoft.com/en-us/azure/azure-app-configuration/overview)
[Azure API Management](https://learn.microsoft.com/en-us/azure/api-management/api-management-key-concepts)
[Azure Managed Redis](https://learn.microsoft.com/en-us/azure/redis/overview)


