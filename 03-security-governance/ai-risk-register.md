# AI Risk Register: Secure Enterprise RAG Assistant

**Section:** 03 — Security Governance
**Review Cycle:** Quarterly
**Owner:** Enterprise Architecture / Information Security

---

## Risk Scoring Model

**Risk Score = Likelihood (1-5) × Impact (1-5)**

| Score Range | Risk Level |
|---|---|
| 1–4 | Low |
| 5–9 | Medium |
| 10–14 | High |
| 15–25 | Critical |

---

## Risk Register

| Risk ID | Description | OWASP Ref | Likelihood | Impact | Inherent Score | Controls | Residual Score | Owner |
|---|---|---|---|---|---|---|---|---|
| RISK-001 | Prompt Injection Attack | LLM01 | 4 | 4 | 16 — Critical | Prompt inspection; system prompt hardening; input limits; output validation; RBAC at retrieval | 6 — Medium | InfoSec |
| RISK-002 | Sensitive Data Leakage via Output | LLM02/LLM06 | 3 | 5 | 15 — Critical | RBAC metadata filtering; output DLP; no PHI in knowledge store; data classification tagging | 6 — Medium | InfoSec / Privacy |
| RISK-003 | Unauthorized Access to AI Services | LLM07/LLM08 | 3 | 4 | 12 — High | Enterprise SSO + MFA; RBAC token validation; no unauthenticated endpoints; network segmentation | 6 — Medium | IAM / Network Security |
| RISK-004 | AI Hallucination — Inaccurate Policy Guidance | LLM09 | 3 | 3 | 9 — Medium | RAG grounds responses in retrieved docs; source citations required; output validation; UI disclaimer | 4 — Low | EA / AI Governance |
| RISK-005 | Unauthorized Document Ingestion | LLM03 | 2 | 4 | 8 — Medium | Governance approval workflow; classification check; AI Ingestion Zone isolation; ingestion audit log | 3 — Low | EA / Content Governance |
| RISK-006 | Lateral Movement to Clinical Systems | — | 2 | 5 | 10 — High | Dedicated AI VLANs isolated from clinical; deny-all firewall rule; SIEM alert on cross-zone traffic | 5 — Medium | Network Security / CISO |
| RISK-007 | Third-Party LLM Vendor Data Exposure | LLM02/LLM06 | 3 | 4 | 12 — High | Vendor data handling agreement; zero-data-retention config; prohibition on training use; vendor assessment | 4 — Low | Vendor Mgmt / Legal |
| RISK-008 | Excessive AI Model Autonomy | LLM08 | 2 | 4 | 8 — Medium | Read-only RAG design; no tool-use authorized in v1.0; human-in-the-loop requirement; ARB approval for expansion | 4 — Low | AI Governance |

---

## Risk Acceptance Criteria

**Medium residual or above requires:**
- Documented compensating controls
- Quarterly review with control effectiveness evidence
- Executive sponsor acknowledgment

**High or Critical residual requires:**
- Immediate escalation to CISO and CTO
- Deployment hold until mitigated to Medium or below

---

*All risk scenarios are based on publicly documented AI security frameworks. All content is synthetic.*