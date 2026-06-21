# Retrieval Security: RBAC-Enforced Knowledge Access

**Section:** 05 — RAG Knowledge Architecture
**Version:** 1.0

---

## Purpose

This document defines the security controls that govern the retrieval layer of the Secure Enterprise RAG Assistant — ensuring that users can only retrieve documents they are authorized to access, that retrieval events are fully auditable, and that the retrieval architecture is resistant to access control bypass.

---

## The Core Security Requirement

> **Zero unauthorized retrievals.** A user must never receive content from a document they are not authorized to access — not through direct query, not through edge cases, not through retrieval ranking artifacts.

This requirement drives every design decision in the retrieval security model.

---

## Pre-Filter vs. Post-Filter: The Critical Design Choice

### Post-Filter Pattern (Rejected)

```
query(vector_search, top_k=10)
→ returns 10 chunks (may include unauthorized documents)
→ filter results by user role
→ deliver authorized subset
```

**Why this is rejected:**
- Unauthorized document content enters the processing pipeline, even if removed before delivery
- Creates timing window where sensitive content is in memory
- Post-filter may reduce result quality (if many results are filtered, top-K may return few or zero authorized results)
- Audit trail is ambiguous — was unauthorized content processed before filtering?

### Pre-Filter Pattern (Implemented)

```
build_filter(user_role → access_tier)
query(vector_search, top_k=10, filter={"access_tier": {"$lte": user_max_tier}})
→ returns only chunks from authorized documents
→ no unauthorized content ever enters the pipeline
```

**Why this is required:**
- Unauthorized documents never enter the retrieval pipeline
- Clean audit trail — retrieved chunks are all authorized
- Consistent result quality — top-K results are all from the authorized corpus
- Defense-in-depth — access control enforced at the database layer, not application layer

---

## RBAC Metadata Filter Implementation

### Filter Construction

When a user submits a query, the RAG engine constructs a metadata filter from the user's RBAC role claims:

```python
# Synthetic example — illustrative only
def build_retrieval_filter(user_role: str) -> dict:
    role_tier_map = {
        "Enterprise_Architect": 3,
        "Security_Architect": 3,
        "Network_Architect": 2,
        "Cloud_Architect": 2,
        "Operations_Lead": 2,
        "All_Approved_Users": 2
    }
    max_tier = role_tier_map.get(user_role, 1)
    return {
        "classification_tier": {"$lte": max_tier},
        "access_tier": {"$in": get_role_categories(user_role)}
    }
```

### Filter Validation

Before any vector search is executed:
1. Filter is constructed from validated RBAC token claims (not user-supplied input)
2. Filter is validated — missing or malformed filter blocks the query
3. Filter is logged with the request — retrieval cannot proceed without a valid filter

**If filter construction fails for any reason — the query is blocked.** There is no fallback to unfiltered retrieval.

---

## Retrieval Audit Trail

Every retrieval event generates a structured log entry:

```json
{
  "event_type": "rag_retrieval",
  "timestamp": "2025-06-15T10:44:33Z",
  "session_id": "sess_[anonymized]",
  "user_role": "enterprise_architect",
  "query_hash": "sha256:[hash]",
  "rbac_filter_applied": true,
  "filter_tier_ceiling": 3,
  "chunks_retrieved": 4,
  "retrieved_doc_ids": ["DOC-2025-0142", "DOC-2025-0089"],
  "retrieval_latency_ms": 187,
  "unauthorized_attempt_detected": false,
  "siem_forwarded": true
}
```

**Key audit fields:**
- `rbac_filter_applied`: Must always be `true` — any `false` value triggers P1 alert
- `unauthorized_attempt_detected`: Flags if filter construction detected an attempt to bypass access controls
- `retrieved_doc_ids`: Complete list of documents returned — enables post-hoc access review

---

## Retrieval Security Controls Summary

| Control | Implementation | Failure Action |
|---|---|---|
| Pre-query RBAC filter | Applied at vector database query layer | Block query if filter missing |
| Filter from token claims only | RBAC filter built from validated IdP token — not user input | Reject query if token invalid |
| Filter validation before query | Filter checked for completeness before submission | Block query if filter invalid |
| Retrieval audit log | Every retrieval logged with filter details and doc IDs | Log failure triggers P2 alert |
| No unfiltered fallback | No code path exists for unfiltered retrieval | N/A — design constraint |
| Retrieval anomaly detection | SIEM alert if retrieval pattern deviates from baseline | P2 alert to InfoSec |
| Quarterly RBAC configuration review | Access tier assignments reviewed against current roles | Findings remediated within 30 days |

---

## Retrieval Quality and Security Metrics

| Metric | Target | Measurement |
|---|---|---|
| Unauthorized retrieval rate | 0 events | SIEM monitoring + quarterly audit |
| RBAC filter applied rate | 100% of queries | Retrieval audit log |
| Retrieval precision | ≥ 85% | Quarterly evaluation against test set |
| Retrieval latency P95 | ≤ 200ms | Observability dashboard |
| False negative rate (relevant doc not retrieved) | ≤ 15% | Quarterly evaluation |

---

## Retrieval Security Testing

| Test | Frequency | Method |
|---|---|---|
| RBAC bypass attempt | Quarterly | Submit query with manipulated role claim — confirm filter enforcement |
| Cross-tier access test | Quarterly | Submit Tier 2 user query for Tier 3 document — confirm denial |
| Filter removal attempt | Quarterly | Attempt to submit query without filter — confirm block |
| Audit log completeness | Monthly | Verify 100% of queries have corresponding retrieval log entry |
| Doc ID accuracy | Quarterly | Verify retrieved doc IDs match authorized document inventory |

---

*All content is synthetic.*