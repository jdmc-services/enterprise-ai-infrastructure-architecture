# SLA / SLO Model: Enterprise AI Infrastructure

**Section:** 06 — Operations and Observability
**Version:** 1.0

---

## Purpose

This document defines the Service Level Objectives (SLOs) and Service Level Agreements (SLAs) for the Secure Enterprise RAG Assistant. SLOs define the internal operational targets the AI Operations team manages to. SLAs define the commitments made to business stakeholders.

---

## SLO vs. SLA Distinction

| Term | Definition | Audience |
|---|---|---|
| SLO (Service Level Objective) | Internal operational target — what the team manages to | AI Operations team |
| SLA (Service Level Agreement) | External commitment — what is promised to business stakeholders | Business owners, users |
| Error Budget | Allowable failure rate before SLO is considered breached | AI Operations team |

SLOs are set more stringently than SLAs to provide a buffer — the team catches degradation before it becomes an SLA breach.

---

## Availability

| Metric | SLO Target | SLA Commitment | Measurement Window |
|---|---|---|---|
| System availability | 99.5% | 99.0% | Monthly |
| Planned maintenance exclusion | Yes — with 48-hour notice | Yes — with 48-hour notice | Per maintenance window |
| Error budget (monthly) | 3.6 hours downtime allowed | 7.2 hours downtime allowed | Monthly |

**Availability calculation:**
```
Availability % = (Total minutes - Downtime minutes) / Total minutes × 100
Monthly total minutes = 43,200
SLO error budget = 43,200 × 0.005 = 216 minutes (3.6 hours)
```

---

## Performance

| Metric | SLO Target | SLA Commitment | Percentile |
|---|---|---|---|
| End-to-end query response time | ≤ 3 seconds | ≤ 5 seconds | P95 |
| AI Gateway response time | ≤ 200ms | ≤ 500ms | P95 |
| RAG retrieval latency | ≤ 200ms | ≤ 500ms | P95 |
| LLM inference latency | ≤ 2 seconds | ≤ 4 seconds | P95 |
| Document ingestion time (per document) | ≤ 30 minutes | ≤ 2 hours | Average |

---

## Quality

| Metric | SLO Target | SLA Commitment | Measurement |
|---|---|---|---|
| Retrieval precision | ≥ 85% | ≥ 80% | Quarterly evaluation |
| Answer groundedness | ≥ 90% | ≥ 85% | Quarterly evaluation |
| Citation accuracy | 100% | 100% | Quarterly audit |
| Unauthorized retrieval rate | 0 events | 0 events | Continuous monitoring |

---

## Security

| Metric | SLO Target | SLA Commitment | Measurement |
|---|---|---|---|
| RBAC filter applied rate | 100% of queries | 100% of queries | Continuous |
| Authentication enforced rate | 100% of requests | 100% of requests | Continuous |
| SIEM log latency | ≤ 60 seconds | ≤ 5 minutes | Continuous |
| Security alert response time (P1) | ≤ 15 minutes | ≤ 30 minutes | Per incident |

---

## Support

| Metric | SLO Target | SLA Commitment |
|---|---|---|
| P1 incident response | 15 minutes | 30 minutes |
| P2 incident response | 30 minutes | 1 hour |
| P3 incident response | 2 hours | 4 hours |
| P4 incident response | Next business day | Next business day |
| Planned maintenance notice | 48 hours | 48 hours |
| New document ingestion (after approval) | Same business day | Next business day |

---

## SLO Reporting

| Report | Frequency | Audience | Owner |
|---|---|---|---|
| Weekly SLO dashboard | Weekly | AI Operations team | AI Operations Lead |
| Monthly SLA report | Monthly | Business stakeholders | AI Operations Lead |
| Quarterly quality review | Quarterly | Enterprise Architecture + Business owners | Enterprise Architect |
| Annual SLA review | Annual | CISO + CTO + Business owners | Enterprise Architect |

---

## SLA Breach Response

If an SLA commitment is breached:

1. AI Operations Lead notified immediately
2. Root cause analysis initiated within 24 hours
3. Business stakeholders notified with impact summary
4. Remediation plan documented within 5 business days
5. SLA breach documented in quarterly governance review

---

*All content is synthetic.*