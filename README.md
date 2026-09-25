# 🛡️ Enterprise SOC Engineering & Incident Response Portfolio

> **Cloud SIEM Architecture | Detection Engineering | SOAR Automation | Threat Hunting | Live Incident Triage**  
> *Platforms:* Microsoft Sentinel, Microsoft Defender XDR, Azure Logic Apps, Azure Log Analytics, KQL, Microsoft Entra ID

---

## 📌 Executive Portfolio Overview

This repository documents two production-grade cybersecurity operations labs designed to mirror the operational cadence of an Enterprise Security Operations Center (SOC Tier 1/2/3). 

Rather than relying purely on pre-canned simulations, this portfolio demonstrates end-to-end cloud security engineering—from building SIEM data pipelines and automating incident triage to identifying, investigating, and reporting an **active, real-world multi-stage credential breach** across an enterprise tenant.

```
                           ┌──────────────────────────────────────────────┐
                           │      Enterprise SOC Operations Portfolio     │
                           └──────────────────────┬───────────────────────┘
                                                  │
                  ┌───────────────────────────────┴───────────────────────────────┐
                  ▼                                                               ▼
   ┌──────────────────────────────┐                                ┌──────────────────────────────┐
   │            LAB 01            │                                │            LAB 02            │
   │  Cloud SIEM Architecture &   │                                │  Threat Hunting, SOAR &      │
   │  Administrative Detection    │                                │  Live Day-0 Incident Triage  │
   ├──────────────────────────────┤                                ├──────────────────────────────┤
   │ • Azure Diagnostic Settings  │                                │ • 8 Live Data Connectors     │
   │ • AzureActivity Ingestion    │                                │ • NRT & Scheduled KQL Rules  │
   │ • KQL Detection Engineering  │                                │ • Logic Apps Managed Identity│
   │ • Entity Mapping & Grouping  │                                │ • 6-Stage MITRE Kill Chain   │
   │ • Investigation Graph        │                                │ • 34 Compromised Accounts    │
   │ • Sabotage/Tampering Triage  │                                │ • 8 Service Principal Backd. │
   └──────────────┬───────────────┘                                └──────────────┬───────────────┘
                  │                                                               │
                  ▼                                                               ▼
        [ Explore Lab 01 ]                                              [ Explore Lab 02 ]
   (./lab-01-sentinel-siem-architecture)               (./lab-02-threat-hunting-soar-incident-response)
```

---

## 📂 Laboratory Structure & Quick Links

| Laboratory | Scope & Competencies | Key Evidence / Artifacts |
|---|---|---|
| **[Lab 01: Cloud SIEM Architecture & Tampering Detection](./lab-01-sentinel-siem-architecture/)** | • Ingestion pipelines via Diagnostic Settings<br>• `AzureActivity` control-plane telemetry<br>• Resource destruction detection (T1485, T1562)<br>• Entity mapping & 5-hour alert grouping<br>• Incident graph investigation | • [Rule KQL Logic](./lab-01-sentinel-siem-architecture/detection-rules/suspicious-resource-deletion.kql)<br>• 6 Step-by-Step Screenshots<br>• [Lab 01 Walkthrough](./lab-01-sentinel-siem-architecture/README.md) |
| **[Lab 02: Threat Hunting, SOAR & Live Day-0 Incident Triage](./lab-02-threat-hunting-soar-incident-response/)** | • Live telemetry correlation (Entra ID, MCAS)<br>• NRT & Scheduled multi-signal detection rules<br>• SOAR Playbook with System-Assigned Managed Identity<br>• Day-0 discovery of 8 Service Principal backdoors<br>• Full 6-phase MITRE ATT&CK kill chain hunting<br>• Formal Responsible Disclosure advisory | • [NRT & Scheduled KQL Rules](./lab-02-threat-hunting-soar-incident-response/detection-rules/)<br>• [7 Threat Hunting Queries](./lab-02-threat-hunting-soar-incident-response/threat-hunting/)<br>• [SOAR Playbook Architecture](./lab-02-threat-hunting-soar-incident-response/automation/playbook-documentation.md)<br>• [Forensic Incident Report](./lab-02-threat-hunting-soar-incident-response/findings/incident-report-anonymized.md)<br>• 19 High-Resolution Screenshots |

---

## 🌟 The Core Story: Star Format (Lab 02 Spotlight)

### 🎯 Situation
While configuring advanced identity threat detection rules in Microsoft Sentinel within an educational cloud tenant to prepare for the Microsoft SC-200 certification, the workspace began streaming high-severity security alerts reflecting active, live-fire intrusion attempts against organizational accounts.

### 📋 Task
Validate whether the telemetry reflected false-positive noise or an active breach. Reconstruct the entire kill chain across all available data providers (Entra ID Protection, Microsoft Defender XDR, Defender for Cloud Apps), engineer automated response controls to triage incoming incidents, and compile an actionable forensic advisory for the institution's Incident Response team.

### 🚀 Action
1. **Engineered Multi-Signal Detection Rules:** Built Near-Real-Time (NRT) and Scheduled correlation rules to isolate credential spray attacks and group multi-vector alerts.
2. **Automated Triage via SOAR:** Deployed an Azure Logic App playbook utilizing a System-Assigned Managed Identity with `Microsoft Sentinel Responder` RBAC permissions, tied to a Sentinel Automation Rule.
3. **Executed Deep KQL Threat Hunting:** Extracted attacker infrastructure across 20+ countries, cataloged 34 confirmed compromised identities (anonymized via SHA-256), discovered Day-0 persistence backdoors in Azure AD Enterprise Applications, and uncovered malware staging in institutional OneDrive repositories.
4. **Coordinated Responsible Disclosure:** Authored and transmitted a structured security advisory to the institution's Digital Security Unit detailing active IoCs, compromised accounts, and containment steps.

### 🏆 Result
* **384 correlated alerts** categorized across 6 MITRE ATT&CK tactical stages.
* **34 confirmed compromised accounts** and **8 Azure AD Service Principal backdoors** isolated and reported.
* **End-to-end automated triage pipeline** active in Microsoft Sentinel.
* Complete alignment with enterprise SOC operational workflows and SC-200 certification competencies.

---

## 🗺️ MITRE ATT&CK Coverage Matrix

| Tactical Phase | Technique ID | Technique Name | Evidence & Telemetry Source |
|---|---|---|---|
| **Initial Access** | `T1110` | Brute Force (Password Spraying) | 145 alerts across 150+ IPs and 20+ countries (Entra ID Protection) |
| **Defense Evasion** | `T1090` | Proxy (Tor / Anonymizer VPNs) | 40 alerts from Tor exit nodes & `Anomalous Token` cookie theft |
| **Persistence** | `T1078` | Valid Accounts (Compromised Logins) | 153 alerts confirming unauthorized access (Atypical Travel, Malicious IPs) |
| **Persistence** | `T1098` | Account Manipulation (Service Principals) | 9 alerts: Python script injected secrets into 8 Azure AD Enterprise Apps |
| **Lateral Movement** | `T1566` | Phishing (Internal Mailbox Abuse) | 32 user complaints from phishing emails sent via internal accounts |
| **Execution** | `T1204` | User Execution (Malware in Cloud Storage) | 5 alerts: Defender for Cloud Apps detected malware in OneDrive |
| **Impact** | `T1485` | Data Destruction (Resource Sabotage) | Automated deletion of ARM resources monitored in `AzureActivity` |

---

## 🔒 Privacy & Responsible Disclosure Notice

* **Anonymization:** All account identities referenced in this public repository have been cryptographically hashed using SHA-256 (first 12 characters, formatted as `acct_[hash]`). Real identities, tenant IDs, and personal identifiable information (PII) are strictly withheld.
* **Attacker Infrastructure:** Attacker IP addresses (e.g., `203.57.82.133`, `98.167.221.41`) and malicious User-Agents (`Python-urllib/3.11`) are published as public Indicators of Compromise (IoCs) to aid community defenders.
* **Ethics:** All actions performed were purely passive and analytical. Zero offensive tools, unauthorized accesses, or system modifications were executed. A formal Responsible Disclosure report was delivered to the affected institution's security team.

---

## 💻 Technical Competencies Demonstrated

`Microsoft Sentinel` `Microsoft Defender XDR` `KQL (Kusto Query Language)` `Detection Engineering` `Threat Hunting` `SOAR Automation` `Azure Logic Apps` `Managed Identity (System-Assigned)` `Azure RBAC Governance` `MITRE ATT&CK Framework` `Entra ID Protection` `Defender for Cloud Apps (MCAS)` `Incident Response (IR)` `Responsible Disclosure` `IoC Extraction` `Alert Grouping & Entity Mapping`
