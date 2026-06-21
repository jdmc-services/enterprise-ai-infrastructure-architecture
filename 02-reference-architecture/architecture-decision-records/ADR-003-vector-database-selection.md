# ADR-003: Vector Database Selection Criteria

**Status:** Decided
**Date:** 2025
**Deciders:** Enterprise Architecture Team
**Section:** 02 — Architecture Decision Records

---

## Context

The Secure Enterprise RAG Assistant requires a vector database to store document embeddings and support semantic similarity search with metadata filtering. The selected vector database must support the RBAC pre-filter retrieval pattern, operate within the enterprise network environment, and meet performance, scalability, and operational requirements.

---

## Decision

**Selected criteria for vector database selection (vendor-neutral)**

This ADR defines selection criteria rather than naming a specific vendor, to allow evaluation against the enterprise's existing technology standards and procurement relationships.

---

## Mandatory Requirements

Any vector database selected must meet all of the following:

| Requirement | Rationale |
|---|---|
| **Pre-query metadata filtering** | RBAC access control requires filtering before vector search — not after. Post-filter retrieval is a security anti-pattern for this use case. |
| **Private deployment option** | Must be deployable within enterprise network environment (on-premise or private cloud). No mandatory SaaS-only deployment. |
| **RBAC-compatible metadata schema** | Must support arbitrary metadata fields including classification tier, access tier, document ID, owner, and version. |
| **High-dimensional vector support** | Must support embedding dimensions matching selected embedding model (typically 768–3072 dimensions). |
| **Query latency** | P95 query latency ≤ 200ms for knowledge base up to 100,000 document chunks. |
| **Index persistence** | Embeddings must persist across restarts without re-indexing requirement. |
| **Backup and restore** | Must support snapshot-based backup and point-in-time restore. |
| **Access control on database** | Database-level access controls (not just application-layer). |
| **Audit logging** | Query and write operations must be loggable for SIEM forwarding. |

---

## Evaluation Criteria (Weighted)

| Criterion | Weight | Description |
|---|---|---|
| Pre-query metadata filtering | 25% | Critical for RBAC security pattern |
| Private deployment | 20% | Required for data residency and zone isolation |
| Performance at target scale | 15% | Query latency and throughput at production load |
| Operational maturity | 15% | Monitoring, backup, restore, upgrade procedures |
| Enterprise support | 10% | Vendor support SLA and enterprise licensing |
| Integration ease | 10% | SDK availability, API compatibility with RAG framework |
| Cost profile | 5% | Licensing and infrastructure cost at target scale |

---

## Disqualifying Factors

The following characteristics disqualify a vector database from selection:

- SaaS-only deployment with no private hosting option
- No support for pre-query metadata filtering
- No enterprise support tier available
- No audit logging capability
- Requires outbound internet access from AI Data Zone for operation

---

## Consequences

**Positive:**
- Vendor-neutral criteria allow selection against enterprise procurement standards
- Security requirements (pre-filter, audit logging) built into selection gate
- Performance requirements ensure production-ready selection

**Negative / Trade-offs:**
- Vendor evaluation required before implementation begins
- Selected database must be validated against embedding model compatibility

---

*All content is synthetic.*