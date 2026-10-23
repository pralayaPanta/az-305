---
tags:
  - az-305
  - studynote
title: Storage - data integration
author: Pralaya Panta
updated: 2026-10-22
---
```table-of-contents
```


## Online Analytical vs Transaction Procession

- OLAP
	- Azure Data Lake
	- Azure Synapse Analytics
	- Tools
		- Azure Synapse Pipelines
		- Data Factory
		- Azure Synapse Pool
		- Databricks
- OLTP
	- Table Storage and Cosmos DB
	- Azure SQL
	- Open source databases
- Streaming data
	- Azure Stream Analytics (semi structured data)
	- Azure Data Explorer (structured data)


##  Azure Data Factory

- supports 100+ connectors, including Microsoft Fabric Warehouse and Fabric Lakehouse
- no need to maintain, configure, or deploy servers
- data driven workflow in Azure Data Factory
	1. connect and collect
	2. transform and enrich
	3. provide continuous integration and delivery (CI/CD) and publish
	4. monitor pipeline for scheduled activities and for any failues
- components
	- pipelines and activities (schedule, monitor, manage)
	- datasets (table, file)
	- linked services (SQL server, hadoop, cluster)
	- data flows 
	- integration runtimes
		- **Azure IR**: serverless cloud-to-cloud data movement and Data Flow execution (supports **Managed VNet** with Private Endpoints)
		- **Self-Hosted IR (SHIR)**: bridges **on-premises or private VNet** data stores to cloud over outbound HTTPS `443` (up to 4 nodes for HA)
		- **Azure-SSIS IR**: dedicated managed cluster for lifting and shifting SQL Server Integration Services (`.dtsx`) packages
- serves
	- big data 
	- relational data warehousing

[Azure Data Factory](https://learn.microsoft.com/en-us/azure/data-factory/introduction)

## Azure Data Lake

- combines a file system with a storage platform to quickly identify insights into your data
- can store any type of data by using the data's native format
- primarily designed to work with Hadoop and all frameworks that use the Apache Hadoop Distributed File System (HDFS) as their data access layer
- supports high throughput for input and output–intensive analytics and data movement
- supports both (evaluated in order: **Azure RBAC first**, then **POSIX ACLs**):
	- **Azure RBAC**: coarse-grained access at storage account or container level (`Storage Blob Data Reader/Contributor/Owner`)
	- **POSIX ACLs**: fine-grained directory/file permissions (`rwx`)
		- **Access ACLs**: control access to a specific file or directory
		- **Default ACLs**: set on a directory only; inherited as a template by newly created child items (does not retroactively change existing children)
- priced at Azure Blob Storage levels
- steps
	- Ingest data
	- access stored data
	- configure access control
- good for storing large volumes of text data

[Azure Data Lake](https://learn.microsoft.com/en-us/azure/storage/blobs/data-lake-storage-introduction)


## Azure Databricks

- fully managed, cloud-based Big Data and Machine Learning platform
- provides data science and engineering teams with a single platform for big data processing and Machine Learning
- an analytics workspace built on Apache Spark
- planes
	- control plane
		- hosts jobs, notebooks with query results and the cluster manager
	- compute plane
		-  runtime clusters that are hosted within the workspace
- environments
	- databricks SQL
	- databricks data science and technology
	- databricks machine learning
- **Unity Catalog**: unified governance for data and AI assets across Databricks workspaces (fine-grained row/column access control, automated lineage, central auditing)
- works with Power BI for fast insights.

[Azure Databricks](https://learn.microsoft.com/en-us/azure/databricks/introduction)
[Explore Azure Databricks](https://learn.microsoft.com/en-us/training/modules/explore-azure-databricks)


## Azure Synapse Analytics

- combines features of big data analytics, enterprise data storage, and data integration
-  supports data ingestion, exploration, transformation, and management, and supports analysis for all BI and machine learning needs
- elements
	- **SQL pool**:
		- **Dedicated SQL Pool**: provisioned MPP data warehouse (60 distributions: Hash, Round-Robin, Replicated; billed by DWUs)
		- **Serverless SQL Pool**: on-demand T-SQL queries directly over Parquet/Delta/CSV/JSON files in ADLS Gen2 (pay per TB processed)
	- **Spark pool**: managed Apache Spark clusters for big data processing
	- **Pipelines**: built-in ADF-powered ETL/ELT engine
	- **Azure Synapse Link**: zero-ETL near-real-time analytics (HTAP) over operational data in Cosmos DB, Azure SQL DB, SQL Server 2022, and Dataverse without impacting transactional workloads
	- **Studio**: unified workspace for data prep, management, and visualization
- includes
	- control node
	- compute node
- uses technology called PolyBase (and `COPY INTO`) that retrieve and query data from relational and non relational sources
- analytical option
	- descriptive - what is happening
	- diagnostic - why is it happening
	- predictive - what likely to happen
	- prescriptive - what needs to be done


## Additional Data Integration, Governance & Analytics Services

- **Azure Event Hubs Capture**:
	- automatically batches and writes streaming Event Hubs data directly to **Blob Storage** or **ADLS Gen2** in **Avro** or **Parquet** format by time/size window with zero code
- **Azure Data Explorer (ADX / Kusto)**:
	- fast, fully managed append-only analytics service for high-volume log, telemetry, and time-series data queried via **KQL**
- **Microsoft Purview**:
	- unified data governance, automated data discovery, sensitive data classification, end-to-end **data lineage** (across ADF, Synapse, Fabric, Power BI), and business glossary
- **Microsoft Fabric**:
	- all-in-one SaaS analytics platform built on **OneLake** (single tenant-wide logical data lake using **Delta-Parquet** format)
	- **OneLake Shortcuts**: zero-copy virtual pointers to existing data in ADLS Gen2, AWS S3, or Dataverse without data movement


## Azure Stream Analytics

- fully managed PaaS offering real-time analytics and complex event-processing engine
- supports processing events in three data formats: 
	- CSV, JSON, and Avro
- concepts
	- data streams
		- continuous data generated by apps, devices, or sensors
		- analysed and actionable insights are extracted
	- event processing
		-  consumption and analysis of a continuous data stream to extract actionable insights
- workflow
	- **ingests data** from Azure Event Hubs (including Azure Event Hubs from Apache Kafka), Azure IoT Hub, or Azure Blob Storage
	- analyse data
		- SQL to filter, sort, aggregate and join streaming data 
	- deliver 
		- alerts and actions using Event Hubs, Service Bus
		- dynamic dashboarding using PowerBI
		- data warehousing
		- storage or archiving
- billing is done by Streaming Units consumed representing CPU and memory resources allocated
- encrypts all incoming and outgoing comms and supports TLS 1.2
[Azure Synapse SQL architecture](https://learn.microsoft.com/en-us/azure/synapse-analytics/sql/overview-architecture)

### Warm vs Cold vs Hot data path


- warm data path
	- supports analysing data as it flows through the system
	- data stream processed in near real time
	- data saved to warm storage and pushed to the analytics clients


- cold path
	- consists of batch layer and serving layer that provide long term view of system
	- support rarely used data - data stored for compliance or legal reasons

- hot path
	- used for processing or displaying data in real time
	- latency-sensitive data results need to be ready in seconds or less

