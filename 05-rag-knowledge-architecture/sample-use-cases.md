# Sample Use Cases: Secure Enterprise RAG Assistant

**Section:** 05 — RAG Knowledge Architecture
**Version:** 1.0

---

## Overview

This document describes representative use cases for the Secure Enterprise RAG Assistant — illustrating how different enterprise roles use the system, what documents they retrieve, and what value the assistant delivers.

All use cases are synthetic. All user names, document titles, and query content are fictional.

---

## Use Case 1: Network Architecture Standards Query

**User Role:** Network Architect
**Scenario:** A network architect is designing a new branch office network and needs to confirm the enterprise standard for VLAN segmentation in branch environments.

**Query:**
> "What is the approved VLAN segmentation standard for branch office deployments, and what are the isolation requirements for IoT devices?"

**Expected retrieval:**
- Network Segmentation Standard v2.3 — Section 3.2 (Branch VLAN Architecture)
- Network Segmentation Standard v2.3 — Section 5.1 (IoT Isolation Requirements)

**Expected response:**
The assistant returns the specific VLAN architecture requirements for branch deployments, including IoT isolation VLAN requirements, firewall zone rules, and inter-VLAN routing restrictions — cited to the specific sections of the approved standard.

**Value delivered:** Network architect gets instant, cited, version-specific guidance without searching SharePoint or emailing the EA team.

---

## Use Case 2: Vendor Access Policy Verification

**User Role:** Enterprise Architect
**Scenario:** An enterprise architect is onboarding a new third-party cloud vendor and needs to confirm the access requirements the vendor must meet before being granted connectivity.

**Query:**
> "What are the access requirements for third-party vendors connecting to our enterprise cloud environment? Specifically around MFA, access logging, and contractual requirements."

**Expected retrieval:**
- Vendor Access Policy v3.0 — Section 2.1 (Authentication Requirements)
- Vendor Access Policy v3.0 — Section 4.2 (Logging and Audit Requirements)
- Vendor Access Policy v3.0 — Section 6.0 (Contractual Obligations)

**Expected response:**
The assistant summarizes vendor MFA requirements, access logging obligations, and required contractual clauses — with citations to the specific policy sections and version number.

**Value delivered:** Consistent policy application across all vendor onboarding. No interpretation variance between team members.

---

## Use Case 3: Incident Response Procedure Lookup

**User Role:** Operations Lead
**Scenario:** An operations lead receives a SIEM alert for an AI system anomaly and needs to quickly confirm the correct incident response procedure.

**Query:**
> "What is the incident response procedure for an AI model anomaly alert? What are the escalation steps and who do I notify first?"

**Expected retrieval:**
- AI Ops Runbook v1.0 — Section 3 (SOP-003: Responding to SIEM Alert)
- Incident Response Playbook v1.0 — Section 2 (P1 Escalation Procedure)

**Expected response:**
The assistant returns the step-by-step response procedure for a P1/P2 AI system alert, including first responder actions, escalation contacts, and documentation requirements — cited to the runbook and playbook.

**Value delivered:** Operations lead has immediate access to the correct procedure during an active incident — no time lost searching documentation systems.

---

## Use Case 4: Cloud Connectivity Standard Confirmation

**User Role:** Cloud Architect
**Scenario:** A cloud architect is evaluating connectivity options for a new cloud-hosted application and needs to confirm the enterprise standard for private cloud interconnect requirements.

**Query:**
> "What are the enterprise requirements for private cloud connectivity? Does the standard require dedicated circuits or is VPN acceptable for production workloads?"

**Expected retrieval:**
- Cloud Connectivity Standard v1.8 — Section 2.1 (Production Connectivity Requirements)
- Cloud Connectivity Standard v1.8 — Section 3.0 (VPN vs. Dedicated Circuit Policy)

**Expected response:**
The assistant returns the specific connectivity requirements for production cloud workloads, including the policy position on VPN vs. dedicated circuit, bandwidth minimums, and redundancy requirements — cited to the standard.

**Value delivered:** Cloud architect gets authoritative, version-specific guidance before making a connectivity architecture decision.

---

## Use Case 5: Change Management Process Query

**User Role:** All Approved Users
**Scenario:** A team member needs to understand the change control process for a firewall rule modification affecting an AI infrastructure zone.

**Query:**
> "What is the change management process for firewall rule changes? Who needs to approve changes to AI zone firewall policies?"

**Expected retrieval:**
- Change Management Policy v2.1 — Section 4.0 (Firewall Change Requirements)
- Change Management Policy v2.1 — Section 5.2 (AI Infrastructure Change Approvals)
- Network Segmentation Standard v2.3 — Section 8.0 (Change Control Requirements)

**Expected response:**
The assistant returns the specific approval requirements for firewall changes, including dual-approval requirements for AI zone changes, the change request process, and post-implementation validation requirements.

**Value delivered:** Team member understands the correct process without escalating to the EA team for a process question.

---

## Use Case 6: Data Classification Policy Lookup

**User Role:** Enterprise Architect
**Scenario:** An enterprise architect is evaluating whether a new document type can be added to the AI knowledge store and needs to confirm the classification requirements.

**Query:**
> "What classification tier applies to security architecture standards? And what approval is required before a Tier 3 document can be ingested into the AI knowledge store?"

**Expected retrieval:**
- Data Classification Policy v1.0 — Section 3.3 (Tier 3 — Confidential)
- Data Classification Policy v1.0 — Section 5.0 (Classification Assignment Process)

**Expected response:**
The assistant confirms security architecture standards are Tier 3 (Confidential), explains the elevated controls required, and describes the executive sponsor approval requirement before Tier 3 ingestion — with citations.

**Value delivered:** Enterprise architect gets a definitive classification answer before submitting a document to the governance workflow.

---

## Boundary Conditions: What the Assistant Will NOT Do

| Request | Response |
|---|---|
| "Show me all documents in the knowledge store" | Declined — out of scope; retrieval is query-based only |
| "Give me the contents of the vendor contract" | Not available — contracts are Tier 4 prohibited content |
| "What is [employee name]'s access level?" | Not available — personnel data not in knowledge store |
| "Help me write a change request" | Out of scope — assistant retrieves standards, does not generate operational documents |
| "What did you say in my last session?" | Not available — no cross-session memory |
| Query without authentication | Blocked at AI Gateway — authentication required |

---

*All content is synthetic. All user names, document titles, and queries are fictional.*