---
tags:
  - studynote
title: Monitoring - Solutions
author: Pralaya Panta
---


```table-of-contents
```



## Azure Monitor

- components
	- Azure Monitor Logs
		- collect and organise data from resources that you monitor
		- gathered and organised on the platform
	- Azure Monitor Metrics
		- captures numerical data from monitored resources 
- data from multiple resources can be collected into Azure Monitor
- log queries can be used for analysis - using KQL
- data collection rules (DCRs)
	- defines what is collected, how to filter and transform it and where to send it
	- collected by AMA agent
		- windows events and performance counter
		- syslog 
		- IIS logs
		- custom logs (text and JSON)

## Log Analytics Workspace

- azure monitor stores log data in log analytics workspace
- container where you can collect & aggregate data
- data in an Azure Monitor Logs workspace is organized into tables
	- each table stores different kind of data
- billing and retention can be set for each workspace
	- retention can be set 30 to 730 days
	- retains data in - interactive retention and long term retention
- used to run **Kusto Query Language (KQL)** queries on logs
- used by services - Monitor, Sentinel, Automation


- log tables
	- multiple tables automatically created to store monitoring data

- data retention in workspace
	- interactive retention
		- retrieve data from table using KQL 
		- available for visualisation, alerts
	- long term retention
		- low cost state with basic data

- workspace patterns
	- dedicated cluster
		- if daily ingestion reaches at least 100 GB, implement a dedicated cluster
		- allows combining data from multiple workspaces in the cluster to reach level of commitment tier
		- capability
			- customer managed keys
			- lockbox
			- double encryption
			- availability zones
			- ingest from azure event hubs
		- commitment tier pricing model
	- centralised - default
		- all subs and workloads send logs into a single central workspace
		- eliminates cross workspace KQL latency
		- pools ingestion volume to reach Commitment Tier faster
	- decentralised - multiple workspaces
		- separate workspace per region or compliance boundary
		- only recommended when required by Data Sovereignty or regulation
		- cross-workspace KQL query - supports up to 100 workspace

- data collection rules 
	- defines what is collected, how to filter and transform it and where to send it
	- methods
		- Azure Monitor Agent
		- Metric export and Diagnostic settings
		- Logs ingestion API

- cost
	- no direct cost for creating workspace
	- charged for ingesting data
	- charged for data retention
	- commitment tiers
		- provide discount to workspace ingestion cost when specific amount of daily data committed
		- can use single workspace to reach the level
		- same volume of data spread across workspace would not be eligible

- data access control
	- resource context RBAC
	- table level RBAC


## Azure insights Insights

- Azure insights provide a customized monitoring experience for particular applications and services.    
- Azure insights collect and analyze both logs and metrics.
- example
	- app insights
	- container insights
	- network insights
	- vm insights
	- storage insights etc

## Azure Data Explorer

- fast and highly scalable data exploration service for log and telemetry data.
- handle multiple data streams, so you can collect, store, and analyse your data from all resources.
- analyse large volumes of diverse data from any data source, such as websites, applications, IoT devices, and more.
- Use Azure Data Explorer for diagnostics, monitoring, reporting, machine learning, and other analytics tasks.


##  Azure Sentinel

- scalable, cloud-native Security Information and Event Management (SIEM) and Security Orchestration Automated Response (SOAR) solution
- detect, investigate, and respond to security threats
- features
	- **Data Collection**: Connects to Azure, Microsoft 365, AWS, on-premises, and third-party sources
	- **Analytics & Detection**: Uses built-in and custom rules to identify threats
	- **Workbooks**: Visual dashboards for monitoring and investigation
	- **Hunting**: Proactively search for threats using KQL queries
	- **Incidents**: Aggregates alerts into incidents for investigation
	- **SOAR Automation**: Automate responses with Logic Apps playbooks
	- **Integration**: Works with Log Analytics Workspace for data storage and querying


[Azure Monitor](https://learn.microsoft.com/en-us/azure/azure-monitor/)
[Azure Monitor Insights](https://learn.microsoft.com/en-us/azure/azure-monitor/visualize/insights-overview)
[Log Analytics Workspace](https://learn.mhttps://learn.microsoft.com/en-us/azure/azure-monitor/logs/log-analytics-workspace-overviewicrosoft.com/en-us/azure/azure-monitor/visualize/insights-overview)