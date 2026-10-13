---
tags:
  - studynote
title: Monitoring - Solutions
author: Pralaya Panta
updated: 2026-09-27
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