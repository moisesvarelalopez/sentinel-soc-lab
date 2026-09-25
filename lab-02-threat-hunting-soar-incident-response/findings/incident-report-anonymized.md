# 📋 Incident Response & Forensic Investigation Report (Anonymized)

**Case Reference:** IR-2026-0924-CRED  
**Classification:** True Positive / Critical Severity  
**Target Organization:** Public Higher Education Institution (`[REDACTED].cr`)  
**Investigator:** Moisés Varela (SOC / Security Analyst)  
**Date of Incident:** 21 September 2026 – 25 September 2026  

---

## 1. Executive Summary
Between 21 September and 25 September 2026, an active, multi-stage credential theft and persistence campaign was identified targeting the cloud tenant of a major public educational institution. 

The intrusion initiated with a distributed global password spray across more than 150 IP addresses, succeeded in compromising at least **34 institutional accounts**, transitioned into internal lateral phishing and malware staging on **Microsoft OneDrive for Business**, and culminated in a Day-0 automated persistence campaign deploying **8 Service Principal backdoors** in Azure AD.

A total of **384 correlated security events** were cataloged and analyzed. A comprehensive responsible disclosure advisory was submitted to the institution's Digital Security Unit to facilitate emergency containment.

---

## 2. Attack Chain Mapping (MITRE ATT&CK)

```
[ Phase 1: Initial Access ]  ──►  [ Phase 2: Defense Evasion ]  ──►  [ Phase 3: Credential Access ]
    T1110 - Password Spray             T1090 - Tor/VPN Proxies             T1078 - Valid Accounts
   150+ IPs | 20+ Countries          Anomalous Session Tokens            34 Compromised Accounts
              │                                                                     │
              ▼                                                                     ▼
[ Phase 4: Persistence ]     ◄──  [ Phase 5: Lateral Movement ]  ◄──  [ Phase 6: Execution / Staging ]
   T1098 - Account Manip.              T1566 - Internal Phishing            T1204 - Malware in OneDrive
  8 Service Principal Backdoors        32 User-Reported Emails             MCAS Incident #756814
  IP: 98.167.221.41 (Python)
```

---

## 3. Forensic Evidence by Phase

### Phase 1: Distributed Initial Access (T1110)
* **Mechanism:** Password Spraying targeting multiple student and staff accounts.
* **Telemetry:** 145 alerts recorded in `SecurityAlert`.
* **Primary Attacker Infrastructure:**
  * `203.57.82.133` (London, United Kingdom) – 10 attack bursts
  * `203.17.244.189` (Amsterdam, Netherlands) – 9 attack bursts
  * `167.148.201.149` (Liberty Lake, United States) – 8 attack bursts

### Phase 2: Defense Evasion & Session Hijacking (T1090 / T1539)
* **Mechanism:** Obfuscation of originating IP address to evade geographic Conditional Access policies.
* **Findings:** 40 events of `Anonymous IP address` (Tor exit nodes / commercial VPNs).
* **Token Abuse:** Event `Anomalous Token` detected on account `acct_00b961ca7975`, indicating session cookie hijacking without re-authentication.

### Phase 3: Confirmed Account Compromise (T1078)
* **Mechanism:** Concurrent sessions from impossible geographic distances (*Atypical Travel*).
* **Scope:** 34 unique institutional accounts demonstrated simultaneous logins from Costa Rica and remote overseas infrastructure (France, Singapore, Croatia, UK).
* **Key Victim Profiles:** Departmental personnel and general student population.

### Phase 4: Day-0 Automated Persistence (T1098)
* **Date & Time:** 24 September 2026, 14:23 UTC to 18:08 UTC.
* **Tooling:** Automated Python script (`User-Agent: Python-urllib/3.11`) executing from dedicated IP `98.167.221.41`.
* **Action:** Injected credentials (certificates/client secrets) into 8 distinct Azure AD Enterprise Applications / Service Principals, immediately establishing API authentication against Azure Resource Manager.
* **Strategic Risk:** Service Principals operate independently of user lifecycle; changing user passwords or enabling MFA does **not** revoke the injected application credentials.

### Phase 5 & 6: Internal Spearphishing & OneDrive Malware Staging (T1566 / T1204)
* **Cloud Storage Abuse:** On 23 September at 01:44 UTC, Defender for Cloud Apps generated Incident ID `756814` for `Malware detection` within `Microsoft OneDrive for Business` under account `acct_1dd5553ab13c`.
* **Pivot Phishing:** Compromised mailboxes were leveraged to dispatch internal spearphishing emails referencing trusted `*.sharepoint.com` / `onedrive` links, resulting in 32 user complaints.

---

## 4. Indicators of Compromise (IoCs)

### Network IoCs (Attacker IPs)
| IP Address | Geolocation | Attribution / Activity |
|------------|-------------|------------------------|
| `98.167.221.41` | United States | Persistence script injector (Service Principals) |
| `203.57.82.133` | London, UK | Primary Password Spray node |
| `203.17.244.189` | Amsterdam, NL | Secondary Password Spray node |
| `167.148.201.149` | Liberty Lake, US | Spray node |
| `43.156.227.68` | Singapore | Cloud proxy infrastructure for confirmed logins |

### Host / Application IoCs
* **Malicious User-Agent:** `Python-urllib/3.11`
* **Affected Service Principal IDs:**
  * `2cc5af32-e811-4b0d-8e42-d5c639968dff`
  * `97c9891b-5d79-47ad-ab48-5abbafcddded`
  * `e27011d6-8a65-406c-b7ff-0dbe4f3c84a2`
  * `79ae1f58-cc44-4b5f-9d65-d2049e213bee`
  * `543fef63-1e11-4c6c-9573-aa2ef39ab96a`
  * `4d9f4dff-5a9e-4e5c-bf68-27e688705628`
  * `f33389dc-c2dc-4cff-b623-6ac5fe2eccd5`
  * `3d3f8a4d-ca2b-4fa3-a367-1b7bfb0fb463`

---

## 5. Containment & Remediation Roadmap
1. **Immediate Revocation:** Audit all Entra ID App Registrations modified on 24 September 2026; delete unauthorized credentials and revoke app permissions.
2. **Identity Hardening:** Invalidate all active user refresh tokens (`Revoke-AzureADUserAllRefreshToken`) for the 34 identified accounts and enforce mandatory FIDO2 / Authenticator MFA.
3. **Perimeter Controls:** Block malicious IPs in Conditional Access Named Locations and perimeter firewalls.
4. **Cloud Storage Quarantine:** Purge malware binaries from the affected OneDrive repository.
