# NIST AI Risk Management Framework Mapping

**Section:** 03 — Security Governance
**Framework:** NIST AI RMF 1.0 (January 2023)

---

## GOVERN Function

| Subcategory | Requirement | Implementation | Evidence |
|---|---|---|---|
| GOVERN 1.1 | Policies for AI risk management exist | Enterprise AI governance policy documented | AI Risk Register; Data Classification Policy |
| GOVERN 1.2 | Accountability for AI risk is assigned | EA team owns AI system; CISO is executive sponsor | RACI matrix |
| GOVERN 2.1 | Teams understand roles and responsibilities | System ownership documented; runbook defines roles | AI Ops Runbook Section 1 |
| GOVERN 4.1 | Organizational risk tolerance established | Risk acceptance criteria documented | AI Risk Register |
| GOVERN 5.1 | Policies address external AI dependencies | Third-party LLM vendor governance policy | RISK-007; Vendor Access Governance |
| GOVERN 6.1 | AI supply chain risks addressed | Vendor security assessment required; contractual data controls | Vendor Access Governance |

---

## MAP Function

| Subcategory | Requirement | Implementation | Evidence |
|---|---|---|---|
| MAP 1.1 | AI system context documented | Use case, users, purpose, and environment defined | Architecture Overview; Executive Summary |
| MAP 2.1 | Technical risk factors identified | Prompt injection, hallucination, data leakage, lateral movement | AI Risk Register |
| MAP 2.2 | Impacts to individuals and organizations assessed | Harm scenarios documented; clinical isolation assessed | RISK-006; RISK-002 |
| MAP 3.1 | AI system components inventoried | Architecture component inventory documented | Architecture Overview |
| MAP 3.5 | Risks from external AI components identified | Third-party LLM vendor risk assessed | RISK-007; ADR-002 |
| MAP 5.1 | Likelihood and impact of risks assessed | Risk register includes full scoring | AI Risk Register |

---

## MEASURE Function

| Subcategory | Requirement | Implementation | Evidence |
|---|---|---|---|
| MEASURE 1.1 | AI risk metrics identified | SLA/SLO model defines performance and quality metrics | SLA/SLO Model |
| MEASURE 2.1 | Test sets reflect real-world use | Synthetic test cases defined for RAG accuracy and security | Demo Lab — test-cases.md |
| MEASURE 2.5 | Performance evaluated against metrics | Retrieval quality scored; hallucination detection active | Logging and Monitoring Model |
| MEASURE 2.6 | Adversarial testing included | Prompt injection test cases documented | Demo Lab; OWASP LLM Controls |
| MEASURE 2.9 | Privacy risks identified and measured | No PHI in knowledge store; output DLP active | RISK-002; Data Classification Policy |
| MEASURE 3.1 | AI risks tracked over time | SIEM integration; quarterly risk register review | Logging and Monitoring Model |

---

## MANAGE Function

| Subcategory | Requirement | Implementation | Evidence |
|---|---|---|---|
| MANAGE 1.1 | Responses to AI risks defined | Controls documented for each risk | AI Risk Register |
| MANAGE 1.3 | Risk response plans include owners | Incident response playbook defines actions and owners | Incident Response Playbook |
| MANAGE 2.2 | AI incidents documented and investigated | Incident logging via SIEM; post-incident review required | Incident Response Playbook |
| MANAGE 2.4 | AI changes follow change control | All changes require Change Request and dual approval | Network Segmentation Model; AI Ops Runbook |
| MANAGE 3.1 | Residual risks tracked | Residual scores maintained; escalation criteria defined | AI Risk Register |
| MANAGE 4.1 | Decommissioning plan exists | AI Ops Runbook includes system retirement section | AI Ops Runbook |

---

## Trustworthiness Characteristics

| Characteristic | Architecture Response |
|---|---|
| **Accountable and Transparent** | Every query and output logged; SIEM integration; source citations in every response |
| **Explainable and Interpretable** | RAG grounds responses in cited source documents |
| **Fair with Managed Bias** | Knowledge store contains only approved, vetted policy documents |
| **Privacy-Enhanced** | No PHI in knowledge store; RBAC-enforced retrieval; output DLP |
| **Resilient** | HA deployment; incident response playbook; RTO/RPO defined |
| **Safe and Secure** | Network isolation; Zero-Trust access; prompt injection controls |

---

*Based on NIST AI RMF 1.0. All content is synthetic.*