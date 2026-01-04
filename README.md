# Azure-Sentinel-Cloud-Security

Hands-on Microsoft Sentinel project monitoring Azure Entra ID sign-ins and audit logs with custom analytics rules, incident investigation, and dashboards for cloud security visibility. This project demonstrates practical cloud security monitoring skills, SIEM rule creation, and SOC incident investigation workflows.

---

## Table of Contents

1. [Project Overview](#project-overview)  
2. [Prerequisites](#prerequisites)  
3. [Project Phases](#project-phases)  
4. [Screenshots](#screenshots)  
5. [KQL Queries](#kql-queries)   

---

## Project Overview

This project simulates a real-world cloud security monitoring environment using **Microsoft Azure Sentinel** and **Entra ID (Azure AD)**. It includes:

- Connecting Azure Entra ID sign-in and audit logs to Sentinel  
- Creating **custom analytics rules** to detect suspicious sign-in behavior  
- Generating and investigating **security incidents**  
- Building **workbooks/dashboards** for SOC visibility  

The project is designed to demonstrate **practical SIEM skills** suitable for resumes, portfolios, or interview discussions.

---

## Prerequisites

- Microsoft Azure account (free tier works)  
- Access to **Microsoft Sentinel**  
- Access to **Azure Entra ID / Azure AD** logs  
- Basic knowledge of **KQL (Kusto Query Language)** is helpful  

---

## Project Phases

### Phase 1 — Workspace Setup
- Created Microsoft Sentinel workspace in **US East**  
- Verified workspace creation with no errors  

**Screenshot:**  
`screenshots/01-workspace-created.png`  

---

### Phase 2 — Connect Entra ID Logs
- Connected **Sign-in logs** and **Audit logs** from Azure Entra ID  
- Verified logs ingestion into Sentinel  

**Screenshot:**  
`screenshots/02-entra-id-logs-connected.png`  

---

### Phase 3 — Verify Log Ingestion
- Ran sample queries to confirm log data was available in Sentinel  
- Connected successfully, logs visible for analytics  

**Screenshot:**  
`screenshots/05-signin-activity-detected.png`  

---

### Phase 4 — Investigate Sign-in Activity
- Queried `SigninLogs` for suspicious sign-ins and authentication details  
- Observed pre-auth throttling and MFA events, demonstrating Entra ID security controls  

**Query Example (KQL):**
```kql
SigninLogs
| summarize AttemptCount = count() by UserPrincipalName
| where AttemptCount >= 3
```

---

### Phase 5 — Custom Analytics Rule
- Created a **Scheduled Query Rule** in Sentinel to detect suspicious multiple sign-in attempts  
- MITRE ATT&CK tactic: **Credential Access**  
- Rule triggered alerts when a user had ≥3 sign-in attempts in 15 minutes  
- Configured SOC best practices:
  - Alert threshold > 0  
  - Event grouping = Single alert  
  - Grouping window = 60 minutes  
  - Re-open closed incidents = Enabled  
  - Incident correlation = Tenant default  
- Generated alerts automatically create incidents  

**Screenshot:**  
`screenshots/06-custom-analytics-rule.png`  

**Query Example (KQL):**
```kql
SigninLogs
| where TimeGenerated > ago(15m)
| summarize AttemptCount = count() by UserPrincipalName
| where AttemptCount >= 3
```

---

### Phase 6 — Incident Investigation
- Triggered an incident by generating multiple sign-ins  
- Investigated incident using Sentinel:
  - Reviewed entities and alert timeline  
  - Used investigation graph to understand activity  
- Closed incident as **Benign Positive** with proper documentation  

**Screenshots:**  
- Incident created: `screenshots/07-incident-created.png`  
- Incident investigation: `screenshots/08-incident-investigation.png`  
- Incident closed: `screenshots/09-incident-closed.png`  

---

### Phase 7 — Workbook Dashboard
- Built **workbook dashboards** to visualize log activity and rule alerts  
- Automatically generated chart showing event counts by table (`AuditLogs`, `SigninLogs`, etc.)  
- Provides SOC visibility for security monitoring and reporting  

**Screenshot:**  
`screenshots/10-workbook-dashboard.png`  

**Example Chart KQL (auto-generated):**
```kql
union withsource=_TableName *
| summarize Count=count() by _TableName
| render barchart
```

---

**Author:** Jaycob White   
**Date:** January 2026
