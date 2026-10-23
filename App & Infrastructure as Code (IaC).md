---
tags:
  - studynote
  - az-305
title: App & Infrastructure as Code (IaC)
author: Pralaya Panta
updated: 2026-10-23
---





```table-of-contents
```



## Tools
- ARM templates
- Bicep language
- Terraform

### Azure Resource Manager templates
- defines infrastructure and configuration for declarative, idempotent deployment
- submitted to Azure Resource Manager and deployed parallelly
- `WhatIf` allows to preview resource creation, modification, or deletion before deployment
- deployment modes
	- Incremental mode (default) - leaves existing resources in the Resource Group untouched if not in the template
	- Complete mode - deletes any existing resources in the Resource Group that are not specified in the template
- can be integrated into multiple CI/CD pipelines
- formats
	- JSON
	- Bicep

### Azure Bicep templates
- domain specific language (DSL) that transpiles into ARM JSON templates
- cleaner syntax, automatic dependency detection, and module reusability
- native to azure ecosystem with zero state file to manage
- can use Bicep CLI to decompile any JSON template into Bicep

### Template Specs & Azure Deployment Stacks
- Template Specs - stores versioned ARM/Bicep templates as first-class Azure resources secured with Azure RBAC for organization-wide sharing
- Azure Deployment Stacks - manages the lifecycle of a collection of Azure resources across scopes as a single atomic unit (replaces Azure Blueprints)
	- denySettings (`denyDelete` or `denyWriteAndDelete`) prevents unauthorized drift or deletion of managed resources
	- actionOnUnmanage (`detachAll` vs `deleteAll` / `deleteResources`) controls whether removed resources are deleted or simply detached when updating the stack


## Azure Automation
- process automation
	- automate frequent, time-consuming, and error-prone cloud management tasks using PowerShell or Python runbooks (supports Hybrid Runbook Worker for on-premises servers)
- configuration management
	- Azure Machine Configuration via Azure Policy
	- Change Tracking and Inventory

## Azure DevOps
- Built-in support for Azure Pipelines, Boards, Repos, Test Plans, and Artifacts.
- Advanced multi-stage YAML release workflows and environment approval gates.

## GitHub Actions
- Seamless integration with GitHub repositories (use Workload Identity Federation / OIDC with Entra ID for secretless Azure deployments).
- Flexible event-driven workflows and a marketplace with pre-built actions.


## Jenkins
- Open-source CI/CD automation server for hybrid or multi-cloud build pipelines.
