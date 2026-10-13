---
tags:
  - studynote
  - az-305
title:
author: Pralaya Panta
updated: 2026-10-01
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
		- app content and config can be swapped between slots
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
			- *isolatedV2*
- OS - Linux or Windows 
- WebJobs
	- feature of Azure App Service that run a program or script in the same instance as a web app
	- no extra cost
	- example - cmd, powershell, bash, node.js, java, python
	-  run background tasks as part of application logic

![[Pasted image 20261004091842.png]]

[Introduction to App Service](https://learn.microsoft.com/en-us/azure/app-service/overview)
## Azure Functions

- Code-first
- run a small piece of code in the cloud, without worrying about the infrastructure
	- can write function code in preferred language
	- charged only for the resources used
- event driven
- consideration
	- avoid large long running functions
		- default time out is 300 seconds for Consumption Plan functions, and 30 minutes for any other plan
	- maximize performance by using a separate storage account for each function application

![[Pasted image 20261004121407.png]]

## Azure Logic App 

- Design-first
- cloud-based platform for creating and running automated _workflows_
	-  workflows are step-by-step processes that integrate your applications, data, services, and systems.
- examples
	- schedule and send email notifications by using Microsoft 365 when a specific event happens
	- route and process customer orders across on-premises systems and cloud services
- considerations
	- If you're building an app with no external connections, Logic Apps is probably not the best option
	- 


[Introduction to Azure Logic Apps](https://learn.microsoft.com/en-us/training/modules/intro-to-logic-apps/)

![[Pasted image 20261004121911.png]]
