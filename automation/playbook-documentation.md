# 🤖 SOAR Automation Architecture: Automated Incident Triage Playbook

## 1. Overview
In a modern enterprise Security Operations Center (SOC), speed and consistency during Tier 1 triage are critical. To eliminate manual initial assessment steps and standardize threat containment notes, an automated response pipeline was engineered using **Microsoft Sentinel**, **Azure Logic Apps**, and **Azure RBAC Managed Identities**.

---

## 2. Architecture & Workflow

```
[ Microsoft Sentinel Incident Trigger ]
                  │
                  ▼
[ Azure Logic App: Playbook-Triage-Comentario ]
                  │ (System-Assigned Managed Identity)
                  ▼
[ RBAC Authorization Check ]
                  │ (Role: "Microsoft Sentinel Responder")
                  ▼
[ Action: Add comment to incident (V3) ]
                  │
                  ▼
[ Sentinel Incident Timeline Updated with Triage Guidance ]
```

---

## 3. Component Configuration

### A. Logic App Playbook (`Playbook-Triage-Comentario`)
* **Trigger:** `Microsoft Sentinel incident` (V2/V3)
  * Unlike alert-based triggers, this operates at the Incident level, preserving correlated multi-alert context.
* **Action:** `Add comment to incident (V3)`
  * **Incident ARM ID:** Dynamically mapped from `Incident ARM ID` provided by the trigger.
  * **Comment Content:** Standardized operational triage note instructing the L1 analyst on immediate containment and validation steps:
    ```
    Automated Triage Note: Please investigate this credential compromise alert.
    Check Azure AD sign-in logs for unexpected locations and verify user MFA status.
    ```

### B. Identity & Access Governance (Least Privilege)
* **Authentication Method:** System-Assigned Managed Identity.
* **Credentials:** Zero hardcoded secrets, API tokens, or service accounts.
* **Role Assigned:** `Microsoft Sentinel Responder`
* **Scope:** Resource Group (`moises`)
* **Technical Design Decision:** 
  * The `Microsoft Sentinel Reader` role is insufficient as it cannot write comments to incident objects.
  * The `Contributor` role is overly permissive and violates least privilege principles.
  * `Microsoft Sentinel Responder` provides the exact permissions required to update incident metadata and post triage comments without granting broad subscription management access.

### C. Sentinel Automation Rule (`Auto-Triage Credential Compromise`)
* **Trigger Condition:** `When incident is created`
* **Filter Conditions:**
  * `Analytic rule name` Contains: `Password Spray` OR `SOC - Multi-Signal Credential Attack Correlation`
* **Action:** `Run playbook` ➔ `Playbook-Triage-Comentario`
* **Order:** `1` (Highest priority execution upon incident generation)

---

## 4. Key Learnings & Exam Alignment (SC-200)
* **Trigger Differentiation:** Incident triggers map directly to incident life cycles; alert triggers create fragmentation if alerts are aggregated.
* **Direction of Permissions:** Managed Identity permissions must be granted **from Sentinel to the Logic App's Identity**, not vice-versa.
* **Automation Rules vs. Playbooks:** Automation rules act as the event routing fabric; Logic Apps execute the procedural logic.
