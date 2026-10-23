---
tags:
  - studynote
  - az-305
title: Governance - Azure Policy
author: Pralaya Panta
updated: 2026-10-23
---
```table-of-contents
```


## Standards and Regulations

- Industry Standard
	- Health Information Trust Alliance (HITRUST)
	- The payment card Industry (PCI) Data security Standard (DSS)
	- System and organisation controls (SOC)
- Government Regulations
	- EU General Data Protection Regulations (GDPR)
	- Federal Risk and Authorisation Management Program (FedRAMP)

### Policy Definition & Policy Initiative

- Policy Definition: a JSON rule consisting of `parameters`, `displayName`, `mode` (`All` vs. `Indexed` for resources supporting tags and location), and a `policyRule` (`if` condition + `then` effect)
- Policy Initiative (Policy Set):
	- a collection of multiple related Policy Definitions grouped together to achieve a single overarching compliance or security goal (e.g., CIS Microsoft Azure Foundations Benchmark, PCI-DSS v4.0, or ISO 27001)
	- simplifies assignment and parameter management: instead of assigning dozens of individual policies to a Management Group, you assign one Initiative and pass shared parameters from the initiative down to member policies

### Scope, Exclusions & Exemptions

- Policy Assignment & Scope:
	- binds a Policy Definition or Policy Initiative to a specific Scope (`Management Group -> Subscription -> Resource Group`)
	- inherited downwards automatically by all child scopes and resources
- Exclusions (`notScopes`):
	- allows excluding specific child Subscriptions, Resource Groups, or individual Resources from a parent policy assignment
- Policy Exemptions:
	- creates a tracked, time-bound waiver (`Mitigated` or `Waiver` category with an expiration date) for a specific resource or hierarchy without modifying the main Policy Assignment

### Azure Policy Effects (Evaluation Order)
- `Disabled`:
	- policy rule is ignored
	- useful for testing or temporarily turning off a single rule inside an Initiative

- `Append`:
	- adds additional fields/properties to the ARM request before creation (e.g., append allowed IP rules to a Storage Account firewall)
	- rejects the request if it conflicts with an existing non-matching value

- `Modify`:
	- adds, updates, or removes tags (or specific properties like `managedIdentity`) during resource creation/update AND supports Remediation Tasks via a Managed Identity to fix existing non-compliant resources

- Tag Inheritance:
	- since tags on Subscriptions or Resource Groups do not inherit automatically to child resources, use built-in `Modify` policies (e.g., Inherit a tag from the resource group if missing) + a Remediation Task to enforce tag inheritance

- `Deny`:
	- blocks the non-compliant ARM resource creation or update request outright (returns `403 Forbidden` before the resource is provisioned)

- `Audit`:
	- allows the resource creation/update to succeed, but marks the resource as Non-Compliant in the Azure Policy compliance dashboard and emits an Activity Log event

- `AuditIfNotExists`:
	- evaluates after resource creation
	- marks the parent resource as Non-Compliant if a required related child/extension resource does not exist (e.g., audit VMs that do not have the Azure Monitor Agent installed)

- `DeployIfNotExists` (DINE):
	- evaluates after resource creation
	- if a required related configuration or child resource is missing, automatically triggers an ARM deployment using a Managed Identity to configure it (e.g., automatically deploy Diagnostic Settings, enable Microsoft Defender for Cloud, or install the Azure Monitor Agent on new VMs); supports Remediation Tasks for existing non-compliant resources

- `DenyAction`:
	- blocks specific actions on existing resources (supports blocking `DELETE` operations on resources with a specific tag)

- `Manual`:
	- used for regulatory compliance controls that cannot be attested automatically via ARM (requires manual attestation of compliance status)


### Azure Machine Configuration (Formerly Azure Policy Guest Configuration)

- extends Azure Policy inside Virtual Machines (Azure VMs and Azure Arc-enabled servers)
- uses the Machine Configuration extension + OS-level PowerShell DSC v3 / Chef InSpec packages to audit or configure OS-level settings (e.g., password complexity, installed software, Windows Registry keys, Linux SSH configuration, expired certificates inside the guest OS)


### Azure Deployment Stacks & Template Specs (Replaces Retired Azure Blueprints)

- Azure Blueprints is deprecated/retired in favor of Azure Deployment Stacks + Template Specs
- Template Specs:
	- stores versioned ARM templates or Bicep modules as first-class Azure resources protected by Azure RBAC for standardized sharing across teams
- Azure Deployment Stacks:
	- manages the lifecycle of a collection of Azure resources deployed via Bicep/ARM as a single atomic unit across Resource Group, Subscription, or Management Group scopes
	- Lifecycle protection (`actionOnUnmanage`): automatically deletes or detaches resources that are removed from the Bicep template on subsequent deployments
	- Deny Settings (`denySettings`): attaches native `denyDelete` or `denyWriteAndDelete` locks directly to all managed resources in the stack (with optional excluded principals/actions) so unauthorized admins cannot drift or delete landing zone infrastructure


### Microsoft Defender for Cloud — Regulatory Compliance

- continuously assesses your Azure, hybrid (Azure Arc), and multi-cloud (AWS/GCP) environment against regulatory standards (Microsoft Cloud Security Benchmark [MCSB], ISO 27001, PCI-DSS, NIST SP 800-53, SOC 2, CIS)
- powered underneath by Azure Policy Initiatives; tracks overall Secure Score and provides 1-click Quick Fix remediation for non-compliant resources


https://learn.microsoft.com/en-us/azure/governance/policy/overview




## Azure landing zone


- proven and flexible architecture for governing, securing, and scaling a multi-subscription Azure environment
- components
	- platform landing zone
	- workload landing zone
- implement two ways
	- accelerators
	- custom build

### CAF Enterprise-Scale Landing Zone Recommended Hierarchy

- Tenant Root Group:
	- avoid assigning policies or RBAC directly at Tenant Root; use a top-level Organization Root Management Group right below it
- Platform Management Group (shared central IT infrastructure):
	- Identity: Domain Controllers, Microsoft Entra Connect / Cloud Sync servers
	- Management: Log Analytics Workspaces, Azure Automation, Microsoft Sentinel
	- Connectivity: Hub VNets / Virtual WAN, ExpressRoute, VPN Gateways, Azure Firewall, Azure DNS Private Resolver
- Landing Zones Management Group (workload subscriptions):
	- Corp: internal line-of-business workloads requiring hybrid connectivity and private IP routing via the Hub
	- Online: internet-facing workloads using public endpoints (Front Door, App Gateway) with no hybrid connectivity to on-premises
- Sandbox Management Group:
	- isolated subscriptions for developer experimentation with strict budget limits and no connectivity to corporate networks
- Decommissioned Management Group:
	- cancelled or retired subscriptions awaiting final deletion (all access and deployments blocked via `Deny` policy)
- Subscription Vending:
	- automated, repeatable process (using Bicep, Terraform, or Azure Deployment Stacks) that provisions a new application landing zone subscription, places it into the target Management Group, assigns RBAC roles, creates the spoke VNet with hub peering, and applies baseline policies automatically

https://learn.microsoft.com/en-us/azure/cloud-adoption-framework/ready/landing-zone/?tabs=conceptual%2Chubspoke