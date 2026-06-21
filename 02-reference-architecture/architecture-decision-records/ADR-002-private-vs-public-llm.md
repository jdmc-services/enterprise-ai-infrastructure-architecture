# ADR-002: Private vs. Public LLM Deployment

**Status:** Decided
**Date:** 2025
**Deciders:** Enterprise Architecture / Information Security / Legal
**Section:** 02 — Architecture Decision Records

---

## Context

The Secure Enterprise RAG Assistant requires an LLM inference endpoint to generate responses from retrieved context. Two deployment models were evaluated: a privately hosted LLM (deployed within the enterprise or private cloud environment) and a governed public LLM API (third-party provider with contractual data controls).

The decision must account for the PHI-adjacent nature of the healthcare environment, data residency requirements, vendor risk, and operational supportability.

---

## Decision

**Selected approach: Governed External LLM API with contractual data controls**

With the following mandatory conditions:
- Zero-data-retention API configuration confirmed in writing
- Contractual prohibition on use of enterprise data for model training
- Data residency requirements confirmed and documented
- Vendor security assessment completed before activation
- Private LLM deployment designated as the preferred long-term target

---

## Evaluation Criteria

| Criterion | Private LLM | Governed External API |
|---|---|---|
| Data residency | Full control — data never leaves enterprise environment | Contractual control — data sent to vendor environment |
| PHI risk | Lowest — no external data transmission | Low with controls — vendor agreement required |
| Infrastructure cost | High — GPU compute, model hosting, MLOps pipeline | Lower — API consumption pricing |
| Operational complexity | High — model updates, scaling, availability management | Lower — vendor manages infrastructure |
| Time to production | Months — infrastructure procurement and setup | Weeks — API integration |
| Model quality | Dependent on selected open-source model | Access to frontier model capabilities |
| Vendor dependency | None — fully self-contained | Dependent on vendor availability and pricing |
| Compliance posture | Strongest — no third-party data sharing | Strong with contractual controls |
| Scalability | Bounded by owned compute | On-demand scaling |

---

## Rationale

The governed external API approach is selected for initial deployment for three primary reasons:

**1. Time to value.**
The organization requires a working system within weeks. Private LLM deployment requires infrastructure procurement, model selection, MLOps pipeline setup, and availability engineering — a multi-month program. The governed API approach allows the governance framework and RAG pipeline to be built and validated while the LLM capability is available immediately.

**2. Data controls are sufficient with contractual enforcement.**
The RAG knowledge store contains only approved, non-PHI architecture standards and policy documents (Tier 1 and Tier 2 content). The risk profile of sending this content to a governed external LLM — with zero-data-retention configuration and contractual training prohibition — is acceptable given the document classification framework.

**3. Operational sustainability.**
A privately hosted frontier LLM requires dedicated GPU infrastructure, model update management, availability engineering, and specialized MLOps capability. The organization's current AI operations team does not have this capability at initial deployment. The external API model allows AI operations maturity to be built before taking on private model hosting complexity.

---

## Mandatory Controls (External API)

These controls are non-negotiable before any external LLM API is activated:

| Control | Verification Method |
|---|---|
| Zero-data-retention configuration | Written confirmation from vendor; API configuration documented |
| Training data prohibition | Contractual clause — reviewed by Legal |
| Data residency confirmation | Vendor documentation — approved geography confirmed |
| Vendor security assessment | InfoSec assessment completed and approved |
| Egress proxy FQDN allowlist | Network team configures approved endpoint only |
| Annual vendor review | Calendar reminder set; owner assigned |

---

## Consequences

**Positive:**
- Faster time to production
- Access to frontier model capabilities
- Lower initial infrastructure cost
- Reduced operational complexity at launch

**Negative / Trade-offs:**
- Data transmitted to vendor environment (mitigated by controls)
- Vendor dependency for availability and pricing
- Long-term cost may exceed private hosting at scale
- Vendor model changes require validation and potential reconfiguration

---

## Future State

Private LLM deployment is the preferred long-term architecture as the organization's AI operations maturity grows. A future ADR will govern the migration from governed external API to private model hosting when:
- GPU infrastructure is available
- MLOps capability is established
- Private model quality meets or exceeds current external API

---

*All content is synthetic.*