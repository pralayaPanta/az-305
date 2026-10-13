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



## Table Storage
- Single region with one optional readable secondary read region for high availability.
- Only primary index on PartitionKey and RowKey. No secondary indexes.
- Applications that are written for Azure Table Storage can migrate to the Azure Cosmos DB for Table with few code changes.


## Cosmos DB
- fully managed NoSQL database service for modern app development
	- takes database administration off your hands with automatic management, updates, and patching
	- handles capacity management with cost-effective serverless and automatic scaling options
- has single-digit millisecond response times and guaranteed speed at any scale.
- compatibility
	- Azure Database for MySQL
	- Azure Database for PostgreSQL
	- Azure Database for MariaDB
	- CosmosDB Cassandra API
	- CosmosDB MongoDB API





https://learn.microsoft.com/en-us/azure/cosmos-db/overview
