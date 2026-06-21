# AI Operations Runbook: Secure Enterprise RAG Assistant

**Section:** 06 — Operations and Observability
**Version:** 1.0
**Owner:** Enterprise Infrastructure / AI Operations Team
**Review Cycle:** Quarterly

---

> **Principle:** No AI system goes into production without a runbook, a defined owner, and a tested incident response procedure.

---

## Section 1: System Ownership

| Role | Responsibility | Escalation Priority |
|---|---|---|
| AI Operations Lead | Day-to-day health, monitoring, Tier 1 response | Primary |
| Enterprise Architect (AI) | Architecture decisions, capacity planning, major changes | Secondary |
| Network Security Engineer | AI zone firewall rules, network incidents | Secondary |
| Information Security | Security incidents, SIEM alerts, policy violations | As needed |
| LLM Vendor Support | Model endpoint issues, API availability | External |
| CISO | Critical security incident escalation | Executive |

---

## Section 2: Daily Health Check Procedures

### AI Gateway
```
Check: Gateway access logs — last 24 hours
Check: Rate limiting rules active
Check: Prompt inspection alert log — investigate flagged events
Check: DLP filter active
Target: 0 unacknowledged security alerts; gateway response < 200ms P95
```

### RAG Engine and Vector Database
```
Check: Execute synthetic test query — verify expected retrieval
Check: RBAC metadata filter active — test with restricted-tier query
Check: Retrieval latency in observability dashboard
Check: Vector database index current (last ingestion timestamp)
Target: Retrieval latency < 500ms P95; unauthorized retrieval = 0 events
```

### LLM Endpoint
```
Check: Submit synthetic prompt — confirm response within SLA
Check: Token consumption vs. 24-hour baseline
Check: Confirm pinned model version unchanged
Check: Cost telemetry — flag if > 110% of daily baseline
Target: LLM response latency < 2000ms P95; cost within baseline ± 10%
```

### Network Zone
```
Check: Firewall log — AI zone cross-traffic review
Check: DENY rules for AI-to-CLINICAL and CLINICAL-to-AI active
Check: Any DENY + LOG events in past 24 hours
Check: AI-MGMT logging agents forwarding to SIEM
Target: 0 unauthorized cross-zone traffic events
```

---

## Section 3: Standard Operating Procedures

### SOP-001: Adding a Document to the Knowledge Base

```
1. Verify governance approval record exists
2. Verify data classification is assigned
3. Assign document ID, version, and metadata tags
4. Submit to ingestion pipeline (parse → chunk → embed → load)
5. Validate: test query surfaces new document for authorized role
6. Validate: document NOT retrieved for unauthorized role
7. Log completion; notify document owner
```

### SOP-002: Removing or Updating a Document

```
If retiring:
  - Remove document chunks from vector store by document ID
  - Archive record with retirement date and reason
  - Log retirement event

If updating:
  - Submit new version through ingestion pipeline (SOP-001)
  - After successful ingestion, retire old version chunks
  - Confirm old version no longer appears in retrieval
  - Log version transition
```

### SOP-003: Responding to a SIEM Alert

```
P1 Critical: Prompt injection confirmed; unauthorized cross-zone traffic; exfiltration pattern
  → Execute Incident Response Playbook immediately

P2 High: Authentication anomaly; rate limit bypass; RBAC violation
  → Investigate and escalate within 30 minutes

P3 Medium: Elevated error rate; unusual query pattern; cost anomaly
  → Investigate and document within 4 hours

P4 Low: Informational threshold breach
  → Log and monitor
```

---

## Section 4: Escalation Matrix

| Severity | Scenario | Escalation Time | Escalate To |
|---|---|---|---|
| P1 | Prompt injection confirmed; clinical zone traffic; exfiltration | Immediate | CISO, CTO, Enterprise Architect |
| P1 | AI system completely unavailable | 15 minutes | Enterprise Architect, Vendor Support |
| P2 | RBAC violation; auth anomaly; rate limit bypass | 30 minutes | InfoSec, Enterprise Architect |
| P3 | Degraded performance; cost anomaly | 2 hours | Enterprise Architect |
| P4 | Minor threshold breach | 4 hours | None required |

---

## Section 5: SLA / SLO Reference

| Metric | SLO Target | Alert Threshold |
|---|---|---|
| System Availability | 99.5% monthly | < 99.5% → P3 |
| Query Response Time (P95) | ≤ 3 seconds | > 3 sec → P3 |
| Unauthorized Retrieval Rate | 0 events | Any event → P1 |
| SIEM Log Latency | ≤ 60 seconds | > 60 sec → P2 |
| Daily Cost vs. Budget | ≤ 110% of baseline | > 110% → P3 |

---

## Section 6: Backup and Recovery

| Component | Backup Frequency | RTO | RPO |
|---|---|---|---|
| Vector Database | Daily full snapshot | 4 hours | 24 hours |
| Document Store | Daily backup | 2 hours | 24 hours |
| AI Gateway Configuration | Version-controlled (Git) | 1 hour | Real-time |
| Ingestion Pipeline | Version-controlled (Git) | 1 hour | Real-time |

---

## Section 7: System Retirement

1. Notify all users 30 days in advance
2. Archive all operational logs per retention policy (minimum 1 year)
3. Securely delete vector database and document embeddings
4. Document data destruction in asset retirement record
5. Remove AI zone firewall rules
6. Reclaim VLANs and IP space
7. Close all vendor contracts; confirm vendor data deletion
8. Archive this runbook with decommission record

---

*All procedures are synthetic and designed for portfolio demonstration purposes.*