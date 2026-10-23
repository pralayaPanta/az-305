---
tags:
  - studynote
  - az-305
title: Compute - Serverless
author: Pralaya Panta
updated: 2026-10-23
---


```table-of-contents
```


## Azure App Service

- build and host web apps, background jobs, mobile backends, and RESTful APIs
- HTTP-based service
- platform as a service (PaaS) environment
	- Azure handles the infrastructure to run and scaling of web apps
- supports development in multiple languages and frameworks
- built-in load balancing and traffic management at global scale with high availability
-  provides built-in authentication and authorization capabilities (Easy Auth) and integrates other sign in providers.
- continuous deployment
	- plan work, collaborate on code, build & deploy using DevOps services
	- deployment slots
		- available in Standard, Premium, or Isolated tier
		- live apps with their own host names
		- app content and config can be swapped between slots (zero-downtime VIP swap + auto-swap + canary traffic routing percentage)
		- validate app changes before swapping the slot into production.
- app service costs
	- pay for Azure compute resources app uses based on App Service plan.
	- tier
		- shared compute
			- each app receives a quota of CPU minutes, so _each app_ is charged for the CPU quota
			- *Free, Shared*
		- Dedicated compute
			- App Service plan defines the number of VM instances that the apps are scaled to, so _each VM instance_ is charged
			- *basic, standard, Prem, PremV2, Prem V3, Prem V4*
		- Isolated
			- App Service Environment defines the number of isolated workers that run your apps, and _each worker_ is charged.
			- *isolatedV2* (ASE v3 - single-tenant VNet-isolated deployment with zero multi-tenant stamp fee)
- OS - Linux or Windows 
- WebJobs
	- feature of Azure App Service that run a program or script in the same instance as a web app
	- no extra cost
	- example - cmd, powershell, bash, node.js, java, python
	- run background tasks as part of application logic


[Introduction to App Service](https://learn.microsoft.com/en-us/azure/app-service/overview)
## Azure Functions

- Code-first
- run a small piece of code in the cloud, without worrying about the infrastructure
	- can write function code in preferred language
	- charged only for the resources used
- event driven (triggers and input/output bindings)
- hosting plans
	- Consumption plan - 
		- classic serverless pay-per-execution, scales to zero, 
		- max 10-minute timeout (default 5 min), 
		- no VNet integration
	- Flex Consumption plan - 
		- modern serverless pay-per-execution + scale-to-zero with VNet integration,
		- selectable memory sizes, and optional Always Ready instances to prevent cold starts
	- Premium plan (Elastic Premium) - 
		- zero cold starts (pre-warmed instances), 
		- VNet integration, 
		- unlimited execution duration (60 min default), and larger compute SKUs (EP1-EP3)
	- Dedicated (App Service) plan - 
		- runs on existing App Service Plan VMs (enable Always On; supports VNet integration and unlimited timeout)
	- Container Apps hosting - 
		- runs packaged Linux container functions inside Azure Container Apps
- Durable Functions (stateful serverless workflows)
	- Function Chaining - runs a sequence of functions in order, passing output of one as input to the next
	- Fan-Out / Fan-In - executes multiple functions in parallel, waits for all to complete, and aggregates results
	- Async HTTP APIs - coordinates long-running operations with external clients via a 202 Accepted status polling endpoint
	- Monitor pattern - recurring flexible polling in a workflow without holding compute active between checks
	- Human Interaction - pauses workflow waiting for an external event (e.g., approval) with an automated escalation timeout timer
- consideration
	- avoid large long running functions
		- default time out is 300 seconds for Consumption Plan functions, and 30 minutes for any other plan
	- maximize performance by using a separate storage account for each function application


## Azure Logic App 

- Design-first
- cloud-based platform for creating and running automated _workflows_
	-  workflows are step-by-step processes that integrate your applications, data, services, and systems (1,000+ managed connectors).
- hosting tiers
	- Consumption (multi-tenant) - serverless pay-per-action execution in shared environment
	- Standard (single-tenant) - runs on dedicated compute (WS1/WS2/WS3), supports VNet integration, Private Endpoints, and both stateful and stateless workflows in the same app
- examples
	- schedule and send email notifications by using Microsoft 365 when a specific event happens
	- route and process customer orders across on-premises systems and cloud services
- considerations
	- If you're building an app with no external connections, Logic Apps is probably not the best option


[Introduction to Azure Logic Apps](https://learn.microsoft.com/en-us/training/modules/intro-to-logic-apps/)

