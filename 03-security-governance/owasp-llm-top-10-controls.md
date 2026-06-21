# OWASP Top 10 for LLM Applications — Control Mapping

**Section:** 03 — Security Governance
**Framework:** OWASP Top 10 for Large Language Model Applications (v1.1)

---

## Control Coverage Summary

| OWASP LLM Risk | Controls Active | Residual Risk |
|---|---|---|
| LLM01 — Prompt Injection | Prompt inspection engine; system prompt hardening; input limits; output validation; indirect injection controls; SIEM alerting | Medium |
| LLM02 — Insecure Output Handling | Output validation service; output DLP scanning; output encoding; response length limits | Low |
| LLM03 — Training Data Poisoning | Document governance approval workflow; data classification requirement; AI Ingestion Zone isolation; ingestion audit log; quarterly knowledge base review | Low |
| LLM04 — Model Denial of Service | Rate limiting at AI Gateway; token consumption limits; cost telemetry and alerting; query complexity limits; autoscaling controls | Low |
| LLM05 — Supply Chain Vulnerabilities | Vendor security assessment; vendor data handling agreement; model version pinning; software composition analysis; plugin governance | Low-Medium |
| LLM06 — Sensitive Information Disclosure | Knowledge base scope control (no PHI); RBAC-enforced retrieval; output DLP; system prompt confidentiality; data classification tagging | Medium |
| LLM07 — Insecure Plugin Design | No plugin architecture in v1.0; plugin extension governance; least-privilege design | Low |
| LLM08 — Excessive Agency | Read-only RAG design; human-in-the-loop requirement; no autonomous execution; capability scope documented | Low |
| LLM09 — Overreliance | Source citation requirement; confidence indicators; UI disclaimer; user awareness training; hallucination monitoring | Low |
| LLM10 — Model Theft | Network isolation of LLM endpoint; system prompt protection; rate limiting and anomaly detection; access logging; Zero-Trust network access | Low |

---

## LLM01 — Prompt Injection (Detail)

**Threat:** Attackers craft inputs that override system prompts, bypass access controls, or cause harmful outputs.

**Controls:**

| Control | Layer |
|---|---|
| Prompt inspection engine — input sanitization and pattern detection | AI Gateway |
| System prompt hardening — version-controlled, immutable at runtime | LLM Inference |
| Input length and format limits | AI Gateway |
| Output validation — scans responses before delivery | Output Validator |
| Indirect injection controls — retrieved content separated from instruction context | RAG Engine |
| SIEM alerting on suspected injection patterns | Observability |

---

## LLM06 — Sensitive Information Disclosure (Detail)

**Threat:** LLM reveals sensitive information through responses from training data or retrieval context.

**Controls:**

| Control | Layer |
|---|---|
| Knowledge base scope — only approved non-PHI documents | Ingestion Pipeline |
| RBAC-enforced retrieval — metadata pre-filter | RAG Engine |
| Output DLP scanning | AI Gateway |
| System prompt confidentiality | LLM Inference |
| Data classification tagging on all documents | Ingestion Pipeline + RAG Engine |

---

## Testing and Validation

- **Quarterly adversarial testing:** Prompt injection scenarios against AI Gateway
- **Annual penetration testing:** AI infrastructure in enterprise pentest scope
- **Continuous monitoring:** SIEM alerting on control-relevant events
- **Change control:** Security review required before any control modification

---

*References OWASP Top 10 for Large Language Model Applications v1.1. All scenarios are synthetic.*