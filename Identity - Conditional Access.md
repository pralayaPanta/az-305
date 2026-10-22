---
tags:
  - studynote
  - az-305
title:
author: Pralaya Panta
updated: 2026-10-21
---

```table-of-contents
```


## Conditional Access

- Zero Trust policy engine that evaluates **Signals** to enforce **Access Decisions**
- licensing requirements
	- **Microsoft Entra ID P1:** required for standard Conditional Access policies (user, group, app, device state, location)
	- **Microsoft Entra ID P2:** required for **Risk-based Conditional Access** (Sign-in Risk and User Risk via Entra ID Protection)
	- **Microsoft Entra Workload ID Premium:** required for Conditional Access policies targeting **Workload Identities** (Service Principals and Managed Identities)
- **Signals evaluated (Assignments & Conditions)**
	- **Users, Groups, or Directory Roles** (always exclude emergency access / break-glass accounts)
	- **Workload Identities** (single-tenant service principals)
	- **Target Cloud Apps or User Actions** (e.g., all resources, specific enterprise apps, registering security information, or Continuous Access Evaluation)
	- **Network / Named Locations** (trusted IPv4/IPv6 CIDR ranges, countries/regions via IP or Authenticator GPS coordinates, Compliant Network checks)
	- **Device Platform & Filter for Devices** (Windows, macOS, iOS, Android, Linux; attribute-based expressions such as `device.trustType -eq "ServerAD"`)
	- **Client Apps** (Browser, Mobile apps and desktop clients, Legacy authentication clients such as Exchange ActiveSync / POP / IMAP / SMTP)
	- **Sign-in Risk & User Risk** (High, Medium, Low — evaluated in real time and offline by Microsoft Entra ID Protection)
- **Access Controls — Grant**
	- **Block access** (overrides all grant policies if matched)
	- **Grant access** (can require one or all of the selected controls):
		- Require multifactor authentication (MFA)
		- Require **Authentication Strength**
		- Require device to be marked as **compliant** (via Microsoft Intune)
		- Require **Microsoft Entra hybrid joined device**
		- Require approved client app or **Intune App Protection Policy**
		- Require password change (for High User Risk remediation)
		- Require Terms of Use (ToU) acceptance
- **Access Controls — Session**
	- **Sign-in Frequency (SIF):** forces re-authentication after a specified time period (hours/days) or **Every time** for high-risk actions
	- **Persistent Browser Session:** controls whether users remain signed in after closing and reopening the browser window (never enable on unmanaged/shared devices)
	- **Use Conditional Access App Control:** routes the session through **Microsoft Defender for Cloud Apps** reverse proxy to monitor in-session behavior and block downloads, cut/copy/paste, or printing in real time
	- **Continuous Access Evaluation (CAE):**
		- enables near real-time token revocation and policy enforcement for supported services (**Exchange Online, SharePoint Online, Microsoft Teams, Microsoft Graph**) instead of waiting for the 1-hour OAuth access token lifetime to expire
		- revokes access immediately on critical events: user account disabled/deleted, password changed/reset, MFA enabled, admin explicitly revokes refresh tokens, high user risk detected, or IP location changes outside a trusted Named Location


## Authentication Strengths

- specifies which exact combinations of authentication methods are permitted to access a target application or privileged role
- **3 Built-in Authentication Strength Tiers**
	- **Multifactor Authentication (Standard):**
		- accepts any MFA method (SMS, Voice call, Microsoft Authenticator push notification, OATH hardware/software tokens, plus all passwordless and phishing-resistant methods)
	- **Passwordless MFA:**
		- requires MFA without typing a password
		- supported methods: **Microsoft Authenticator (Passwordless Phone Sign-in)**, **Windows Hello for Business**, **FIDO2 Security Keys / Passkeys**, and **Microsoft Entra Certificate-Based Authentication (CBA)**
	- **Phishing-Resistant MFA (Strongest — Recommended for Admins & High-Privilege Apps):**
		- requires cryptographic verification of the relying party / domain so credentials cannot be intercepted by AitM (Adversary-in-the-Middle) reverse-proxy phishing frameworks
		- supported methods:
			- **FIDO2 Security Keys / Passkeys** (device-bound or synced)
			- **Windows Hello for Business** (TPM-backed asymmetric keys)
			- **Microsoft Entra Certificate-Based Authentication (CBA)** (X.509 client certificates configured as multi-factor)
- **Custom Authentication Strengths:**
	- administrators can define custom combinations of allowed authentication methods (e.g., Temporary Access Pass [TAP] only for initial FIDO2 key bootstrap/onboarding)


[What is Conditional Access in Microsoft Entra ID?](https://learn.microsoft.com/en-us/entra/identity/conditional-access/overview)
[Authentication strengths in Conditional Access](https://learn.microsoft.com/en-us/entra/identity/conditional-access/concept-authentication-strengths)
[Continuous access evaluation in Microsoft Entra ID](https://learn.microsoft.com/en-us/entra/identity/conditional-access/concept-continuous-access-evaluation)
