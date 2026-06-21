# RAG Design Pattern: Secure Enterprise Knowledge Retrieval

**Section:** 05 — RAG Knowledge Architecture

---

## Why RAG in This Environment

RAG grounds the model's responses in a curated, governed, and classified enterprise knowledge base rather than relying on the model's internal training knowledge.

| Consideration | RAG | Fine-Tuning |
|---|---|---|
| Knowledge update frequency | Real-time via ingestion pipeline | Requires model retraining cycle |
| Access control granularity | RBAC enforced at retrieval layer | No native access control in model weights |
| Auditability | Every retrieval logged with document ID | No retrieval event to log |
| Data governance | Documents reviewed and approved before ingestion | Training data governance requires full ML pipeline |
| Hallucination risk | Grounded in retrieved content | Model may generate from training data |
| Time to production | Weeks | Months |

---

## Critical Design Requirement: Filter-First Retrieval

```
CORRECT:   query(vector, filter={"access_tier": user_role})  → returns only authorized docs
INCORRECT: query(vector) → filter results by user_role → deliver authorized subset
```

The RBAC metadata filter must be applied as a **pre-filter** at the vector database query level — not post-filter on results. Post-filtering creates a timing window where unauthorized document content enters the processing pipeline.

---

## End-to-End RAG Flow

```
User Query
  → Authentication check (session valid? RBAC role confirmed?)
  → Prompt inspection (injection check; length validation)
  → Query embedding (query → vector representation)
  → Metadata filter assembly (build filter from RBAC role)
  → Vector similarity search (pre-filtered)
  → Top-K chunks returned with document ID, title, classification, version
  → Context assembly (system prompt + retrieved context + user query)
  → LLM generation (response grounded in retrieved content)
  → Output validation (DLP + quality + confidence)
  → Response delivery with citations
  → Structured log entry → SIEM
```

---

## Context Assembly Structure

```
SYSTEM CONTEXT (pre-approved, version-controlled):
  "You are an enterprise architecture assistant. Answer questions based only
   on the provided context documents. Cite the source document for each claim.
   If the answer is not in the context, say so."

RETRIEVED CONTEXT:
  [Document 1: Network Segmentation Standard v2.3, Section 4.1]
  [Document 2: Cloud Connectivity Standard v1.8, Section 2.2]

USER QUERY:
  "What are the firewall requirements for third-party vendor connections?"
```

---

## Knowledge Base Design

### Document Taxonomy

| Category | Access Tier |
|---|---|
| Network Standards | Enterprise_Architect, Network_Architect |
| AI Governance Policies | All_Approved_Users |
| Cloud Connectivity Standards | Enterprise_Architect, Cloud_Architect |
| Change Management Procedures | All_Approved_Users |
| Vendor Access Policies | Enterprise_Architect, Security_Architect |
| Incident Response Procedures | Operations, Security, Architecture |
| Security Standards | Security_Architect, Enterprise_Architect |

---

## Chunking Strategy

| Parameter | Value | Rationale |
|---|---|---|
| Chunk size | 512 tokens | Balances semantic coherence with retrieval precision |
| Chunk overlap | 50 tokens | Preserves context across chunk boundaries |
| Chunking method | Recursive text splitting with section boundary detection | Respects document structure |
| Metadata preserved | Document ID, title, section, page, classification tier | Enables accurate citation and access filtering |

---

## Retrieval Quality Metrics

| Metric | Target |
|---|---|
| Retrieval Precision | ≥ 85% |
| Retrieval Recall | ≥ 80% |
| Answer Groundedness | ≥ 90% |
| Latency (P95) | ≤ 3 seconds |
| Unauthorized Retrieval Rate | 0% (zero tolerance) |

---

*All content is synthetic.*