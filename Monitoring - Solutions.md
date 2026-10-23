---
tags:
  - studynote
  - az-305
title: Monitoring - Solutions
author: Pralaya Panta
updated: 2026-10-23
---


```table-of-contents
```

## Log Sources & Layers in Azure

- Tenant Logs (Microsoft Entra ID)
	- sign-in logs, audit logs, provisioning logs, risky users
	- routed via Entra ID Diagnostic Settings to Log Analytics, Storage Account, or Event Hubs

- Subscription Logs (Azure Activity Log)
	- control-plane operations (`Write`, `Delete`, `Action` on ARM resources), Service Health, Resource Health, Autoscale events
	- kept for 90 days free in the Azure Portal
	- routed via Activity Log Diagnostic Settings for >90 days retention, alerting, or SIEM export

- Resource Logs (Data Plane)
	- operations performed within an Azure resource (e.g., Key Vault secret reads, Storage blob access, NSG/VNet flows, SQL queries)
	- not collected by default
		- must enable Diagnostic Settings on each resource (or enforce at scale via Azure Policy `DeployIfNotExists`)

- Guest OS Logs (Virtual Machines)
	- Windows Event Logs, Linux Syslog, Performance Counters, IIS logs, Custom text/JSON logs
	- collected and routed via Azure Monitor Agent (AMA) using Data Collection Rules (DCRs)


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
- used to run Kusto Query Language (KQL) queries on logs
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

- data export
	- continuously exports selected tables directly from a Log Analytics Workspace as data arrives (without running KQL queries)
	- destinations
		- Storage Account: exported in hourly batches into append blobs (long-term compliance archive)
		- Event Hubs: exported in near real-time (forward aggregated workspace tables to an external SIEM)



## Diagnostic Settings

- defines where platform metrics, resource logs, and Activity Logs are sent
- up to 5 diagnostic settings per resource
- destinations
	- Send to Log Analytics Workspace
		- use for interactive KQL querying, Log Search Alerts, Workbooks, and Microsoft Sentinel correlation
		- workspace can be in any region
	- Archive to a Storage Account
		- use for low-cost long-term cold/archive compliance retention or immutable WORM storage (Time-based retention / Legal Hold)
		- regional constraint: Storage Account must be in the same Azure region as the monitored resource (for resource logs)
	- Stream to an Azure Event Hub
		- use for real-time streaming to external third-party SIEMs (Splunk, QRadar, Datadog) or custom stream processing (Stream Analytics / Functions)
	- Send to Partner Solution
		- native direct integration with supported third-party observability partners (e.g., Datadog, Dynatrace, Elastic)

## Azure Monitor Insights

- Azure insights provide a customized monitoring experience for particular applications and services.
- Azure insights collect and analyze both logs and metrics.
- example
	- app insights
		- Workspace-based (stores telemetry in `AppTraces`, `AppRequests`, `AppDependencies`, `AppExceptions` tables inside Log Analytics)
		- Auto-instrumentation (codeless agent on App Service / AKS / VMs) vs. OpenTelemetry SDK (custom code telemetry)
		- Distributed Tracing & Application Map: visualizes end-to-end dependency calls and latency bottlenecks across microservices
	- container insights
		- monitors AKS and ACA cluster performance, pod/node logs (`ContainerLogV2`), and integrates with Managed Prometheus & Azure Managed Grafana
	- network insights
	- vm insights
		- uses AMA + Dependency Agent to monitor guest OS performance (CPU, memory, disk) and map TCP network dependencies between servers
	- storage insights etc


## Azure Data Explorer

- fast and highly scalable data exploration service for log and telemetry data.
- handle multiple data streams, so you can collect, store, and analyse your data from all resources.
- analyse large volumes of diverse data from any data source, such as websites, applications, IoT devices, and more.
- Use Azure Data Explorer for diagnostics, monitoring, reporting, machine learning, and other analytics tasks.

## Azure Monitor Alerts & Action Groups

- Alert Rules (4 Core Types)
	- Metric Alerts
		- near real-time alerting on numeric platform or custom metrics (evaluated every 1 minute)
		- supports Static thresholds (fixed value) and Dynamic thresholds (ML-driven seasonal baseline)
		- stateful by default (auto-resolves when metric returns to normal)
	- Log Search Alerts
		- runs scheduled KQL queries against a Log Analytics Workspace (every 1m, 5m, 15m, etc.)
		- can be stateful (1 alert per condition until resolved) or stateless (fires every evaluation interval)
		- higher cost and slightly higher latency than Metric Alerts
	- Activity Log Alerts
		- triggers on control-plane ARM events (e.g., VM deleted, NSG modified), Service Health incidents, Resource Health changes, or Autoscale events
		- free to create and evaluate
	- Prometheus Alerts
		- Prometheus Rule Groups evaluated against Azure Monitor Managed Service for Prometheus (AKS / container metrics)

- Action Groups
	- reusable list of notification channels and automated responses triggered when an alert fires
	- notifications: Email, SMS, Voice, Azure Mobile App Push
	- automation / integration: Azure Functions, Logic Apps, Automation Runbooks, Event Hubs, Webhook, ITSM connector (ServiceNow)
- Alert Processing Rules
	- modifies fired alerts before or instead of calling Action Groups
	- key use cases:
		- suppression during planned maintenance windows
			- e.g., mute VM alerts Sunday 02:00–04:00 AM
		- add Action Groups at scale across an entire subscription or resource group without editing individual alert rules


## Network Watcher & Flow Logs

- VNet Flow Logs (Replaces legacy NSG Flow Logs — retiring June 30, 2027)
	- captures IP traffic flowing through a Virtual Network and writes JSON flow records to an Azure Storage Account
	- Traffic Analytics: analyses VNet Flow Logs in Log Analytics to visualize top talkers, malicious IP flows, open ports, and geo-location maps

- Connection Monitor
	- continuous synthetic network connectivity, latency, and packet-loss monitoring between Azure VMs/scale sets and on-premises hosts (uses AMA extension)

- Diagnostic Troubleshooting Tools
	- IP Flow Verify: checks if a packet is allowed or denied by an NSG/AVNM security rule
	- Next Hop: validates the effective route table (System, BGP, or UDR) for a target destination IP
	- VPN Troubleshoot / Packet Capture: diagnoses gateway tunnels and captures VM NIC packets

## Microsoft Sentinel

- scalable, cloud-native Security Information and Event Management (SIEM) and Security Orchestration Automated Response (SOAR) solution
- detect, investigate, and respond to security threats
- features
	- Data Collection: Connects to Azure, Microsoft 365, AWS, on-premises, and third-party sources
	- Analytics & Detection: Uses built-in and custom rules to identify threats
	- Workbooks: Visual dashboards for monitoring and investigation
	- Hunting: Proactively search for threats using KQL queries
	- Incidents: Aggregates alerts into incidents for investigation
	- SOAR Automation: Automate responses with Logic Apps playbooks
	- Integration: Works with Log Analytics Workspace for data storage and querying

## Microsoft Defender for Cloud
- CSPM (Cloud Security Posture Management): Continuous Secure Score, regulatory compliance benchmarks (CIS, NIST, ISO, PCI-DSS), attack path analysis
- CWPP (Cloud Workload Protection): Defender plans for Servers, SQL, Storage, Containers, Key Vault, App Service, and APIs

## Quick Links
- [Azure Monitor](https://learn.microsoft.com/en-us/azure/azure-monitor/)
- [Azure Monitor Insights](https://learn.microsoft.com/en-us/azure/azure-monitor/visualize/insights-overview)
- [Log Analytics Workspace](https://learn.microsoft.com/en-us/azure/azure-monitor/logs/log-analytics-workspace-overview)
- [Diagnostic Settings in Azure Monitor](https://learn.microsoft.com/en-us/azure/azure-monitor/essentials/diagnostic-settings)
- [Data Collection Rules (DCRs) Overview](https://learn.microsoft.com/en-us/azure/azure-monitor/essentials/data-collection-rule-overview)
- [Log Analytics Workspace Data Export](https://learn.microsoft.com/en-us/azure/azure-monitor/logs/logs-data-export)
- [VNet Flow Logs in Azure Network Watcher](https://learn.microsoft.com/en-us/azure/network-watcher/vnet-flow-logs-overview)



