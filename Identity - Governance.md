---
tags:
  - studynote
  - az-305
title:
author: Pralaya Panta
updated: 2026-10-17
---

```table-of-contents
```



## Entitlement Management (Access Packages)

- bundles multiple resources (**Entra Security Groups, M365 Groups/Teams, Enterprise Apps, SharePoint Online sites**) into a single governable **Access Package** inside a **Catalog**
- supports internal employees and external partner organizations (**Connected Organizations**)
- enforces **multi-stage approval workflows**, **requestor justification**, and **automatic time-bound expiration** (can automatically remove guest accounts when their last Access Package expires)


## Access Reviews
- ensure the correct users have the correct access to the correct resources
- scheduled recurring recertification of group memberships, application access, or Entra / Azure RBAC roles
- reviewers can be Resource Owners, User's Manager, Selected Users, or Self-Review
	- resource owner - the business owner of resource
	- delegates - a group of individuals selected by the access reviews admin
	- end user - a user who self attests to their need
- auto-apply results to resource: automatically revokes access if reviewer denies or fails to respond before the review deadline


## Privileged Identity Management (PIM) (Requires Entra ID P2)
- provides Just-In-Time (JIT) privileged access to Microsoft Entra Roles, Azure RBAC Resource Roles, and PIM for Groups
- converts standing `Active` admin assignments into `Eligible` assignments
- Activation controls: require MFA / Phishing-Resistant Authentication Strength, Business Justification, Ticket Number, Approval Workflow, and Maximum Activation Duration (e.g., 1–8 hours)

## Lifecycle Workflows (Joiner – Mover – Leaver)
- automates identity tasks triggered on HR attribute schedules (e.g., 7 days before `employeeHireDate` generate a Temporary Access Pass [TAP] and email manager; on `employeeLeaveDate` revoke all refresh tokens, remove group/license memberships, and delete account after 30 days)



- [What is Microsoft Entra ID Governance?](https://learn.microsoft.com/en-us/entra/id-governance/identity-governance-overview)
