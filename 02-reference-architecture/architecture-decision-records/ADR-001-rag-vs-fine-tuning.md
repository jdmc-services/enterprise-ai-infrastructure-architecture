# ADR-001: RAG vs. Fine-Tuning for Enterprise Knowledge Retrieval

**Status:** Decided
**Date:** 2025
**Deciders:** Enterprise Architecture Team
**Section:** 02 — Architecture Decision Records

---

## Context

The Secure Enterprise RAG Assistant requires a mechanism to ground AI responses in approved enterprise knowledge — architecture standards, policies, and procedures. Two primary approaches were evaluated: Retrieval-Augmented Generation (RAG) and model fine-tuning.

The decision must account for the regulated, mission-critical healthcare environment, including data classification requirements, access control needs, auditability, and operational supportability.

---

## Decision

**Selected approach: Retrieval-Augmented Generation (RAG)**

---

## Evaluation Criteria

| Criterion | RAG | Fine-Tuning |
|---|---|---|
| Knowledge currency | Real-time via ingestion pipeline | Requires full retraining cycle |
| Access control granularity | RBAC enforced at retrieval layer per query | No native per-user access control in model weights |
| Auditability | Every retrieval logged with document ID and user identity | No retrieval event — model generates from internalized knowledge |
| Data governance | Documents reviewed, classified, and approved before ingestion | Training data governance requires full ML pipeline and data lineage |
| Hallucination risk | Grounded in retrieved content — lower hallucination risk | Model may generate plausible but incorrect content from training data |
| Regulatory compliance | PHI and sensitive data excluded by classification policy | Training data governance burden; risk of sensitive data in model weights |
| Infrastructure complexity | Vector database + ingestion pipeline | GPU compute for training; model versioning; evaluation pipeline |
| Time to production | Weeks (pipeline + governance setup) | Months (training + evaluation + safety testing + deployment) |
| Knowledge update speed | Hours (new document ingested via pipeline) | Weeks to months (retrain, evaluate, deploy new model version) |
| Cost profile | Inference cost + vector DB hosting | Training compute cost + ongoing inference cost |
| Vendor dependency | Embedding model + LLM endpoint (separable) | Tied to specific model version trained on specific data |

---

## Rationale

RAG is the correct architectural choice for this use case for five primary reasons:

**1. Access control is non-negotiable.**
In a regulated environment, different users have different authorization levels. RAG enforces RBAC at the retrieval layer — each query only returns documents the requesting user is authorized to access. Fine-tuning cannot replicate this; the model's internalized knowledge cannot be partitioned by user role.

**2. Auditability is required.**
Every AI interaction must be traceable to a specific source document. RAG retrieval events are logged with document ID, user identity, and query context. Fine-tuning produces no retrieval event — the model generates from internalized training, making source attribution impossible.

**3. Knowledge currency matters.**
Enterprise architecture standards and policies are updated regularly. RAG allows new documents to be ingested within hours of approval. Fine-tuning requires a full retraining cycle — weeks to months — making it impractical for a frequently updated knowledge base.

**4. Data governance is simpler.**
RAG's knowledge base is a curated, classified, approved document store. Governance is applied at the document level before ingestion. Fine-tuning requires governing the entire training dataset — a significantly more complex data lineage and compliance challenge in a PHI-adjacent environment.

**5. Time to production favors RAG.**
The organization needs a working system in weeks, not months. RAG's infrastructure (vector database, ingestion pipeline, embedding service) can be stood up and governed in a fraction of the time required for a fine-tuning pipeline.

---

## Consequences

**Positive:**
- RBAC-controlled retrieval satisfies access governance requirements
- Full retrieval audit trail satisfies auditability requirements
- Knowledge base can be updated without model retraining
- Lower hallucination risk due to grounding in retrieved content
- Faster time to production

**Negative / Trade-offs:**
- RAG responses are bounded by retrieval quality — if relevant documents are not in the knowledge store, the model cannot answer
- Retrieval latency adds to overall response time
- Vector database requires ongoing maintenance and capacity management
- Embedding model version changes require re-embedding the entire knowledge base

---

## Alternatives Considered

**Fine-tuning:** Rejected — see evaluation above. Primary blockers: no per-user access control, no retrieval audit trail, impractical knowledge update cycle.

**Hybrid (RAG + fine-tuning):** Deferred. Could be reconsidered in a future phase if the base model requires domain adaptation beyond what RAG retrieval provides. Would require a separate ADR.

**Pure prompt engineering (no retrieval):** Rejected — model's training knowledge is not current, not enterprise-specific, and not auditable.

---

*All content is synthetic.*