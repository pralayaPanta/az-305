---
tags:
  - studynote
  - az-305
title:
author: Pralaya Panta
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
		 - part of messaging infrastructure
		 -  audit trail of all messages that pass through the queue
		 - if queue does not exceed 80GB
-  Azure queue storage
		 - storage infrastructure

### Azure Service Bus
- fully managed enterprise message broker
- receives messages without polling
- FIFO delivery and automatic duplicate detection
- transactional behaviour and atomicity
- supports 
	- message queues and 
	- publish-subscribe topics.

#### Service Bus Topics (publish-subscribe topics)
- can have multiple receivers to handle each message
- can have multiple destinations for a single message but with queue like behavior

#### Service Bus Queues (message queues)
- groups messages into transactions
- receives messages without polling
- queues can handle messages larger than 64KB and less than 80GB
- at most once delivery, at least once processing, at most once processing of message

## Event Driven Application
- An event is a lightweight notification that indicates something occurred.
- An event can be sent to multiple receivers or to none.

### Azure Event Hub
- fully managed, big data streaming platform and event ingestion service
- can send and receive events in diff language
- events received are added to the end of its data stream
- pull model
	- holds each message in its cache and allows it to be read
	- when message is read it is not deleted
	- messages remain for other consumers
- tiers
	- basic
	- standard
	- premium
- use case
	- live dashboarding, 
	- supporting analytics pipelines like clickstreams, and 
	- detecting anomalies like fraud or outlier actions.
- doesn't handle messages that aren't processed as expected

### Azure Event Grid
- event routing and automation
- aggregates all events and provides routing from src to dst
- distributes event from sources like Azure Blob storage
- distributed to handlers like Az function and webhooks 
	- event handler --> subscribes
- supports
	- MQTT and HTTPs protocol
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
- securely managing IoT devices
- device management
- telemetry integration


## Azure API Management
* **API Gateway:** Acts as a unified front for all APIs.
* **Security Features:** Supports OAuth, JWT, and IP filtering.
* **Rate Limiting and Throttling:** Prevent abuse and ensure stability.
* **Developer Portal:** Self-service portal to discover, test, and interact.
* tiers
	* Classic (dev, basic, standard, premium)
	* V2 tier (basic v2, standard v2, premium v2)
	* Consumption tier
	* Premium v2


## Azure Cache for Redis

- **Definition:** A fully managed, in-memory cache that enables high-performance data storage and retrieval.    
- **Key Features:**    
    - High Throughput        
    - Flexible Data Structure
    - Data Persistence
- Caching Patterns
	- Cache Aside - to only load data into the cache as needed
	- Write-Through
	- Read-Through
- Azure Managed Redis is current recommended caching service
- Azure Managed Redis can help improve performance in apps that interface with many database solutions, including Azure SQL Database, Azure Cosmos DB, and Azure Database for MySQL.



## Azure App Configuration

- centrally manage application settings and feature flags
- offers flexible key representations and mappings, and configuration snapshots - immutable point-in-time captures of all settings used for deployment rollback and audit
- dedicated UI for feature flag management, and supports resource tagging with labels
- sensitive info can be encrypted at rest and in transit
- geo-replication allows App Configuration stores to be replicated across Azure regions
- development
	- visual stuido, code, Azure CLI
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


