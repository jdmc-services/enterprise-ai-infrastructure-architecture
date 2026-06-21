# Hybrid Cloud Connectivity: Enterprise AI Infrastructure

**Section:** 02 — Reference Architecture
**Version:** 1.0

---

## Purpose

This document defines the hybrid cloud connectivity model for the enterprise AI infrastructure — covering how AI workloads connect across on-premise data centers, private cloud environments, and governed external AI endpoints while maintaining security, performance, and governance requirements.

---

## Connectivity Architecture Overview

The enterprise AI infrastructure operates in a hybrid model:

| Component | Deployment Location |
|---|---|
| AI Gateway | On-premise / Private cloud DMZ |
| RAG Engine | On-premise / Private cloud AI Processing Zone |
| Vector Database | On-premise / Private cloud AI Data Zone |
| Document Ingestion Pipeline | On-premise / Private cloud AI Ingestion Zone |
| LLM Inference Endpoint | Private cloud OR governed external API |
| Observability Platform | On-premise / Private cloud AI Management Zone |
| Enterprise IdP | On-premise (existing enterprise infrastructure) |
| Enterprise SIEM | On-premise (existing enterprise infrastructure) |

---

## Connectivity Patterns

### Pattern 1: On-Premise to Private Cloud (Primary)

For organizations with private cloud infrastructure:

```
On-Premise Enterprise Network
  → Private Cloud Interconnect (dedicated circuit / private peering)
  → Private Cloud AI Environment
      → AI Processing Zone (RAG Engine, LLM)
      → AI Data Zone (Vector Database)
      → AI Management Zone (Observability)
```

**Connectivity requirements:**
- Dedicated private circuit (not public internet path)
- Minimum bandwidth: sized to AI workload traffic baseline + 30% headroom
- Latency target: < 10ms between on-premise AI Gateway and cloud AI processing
- Redundant paths: dual circuits from separate providers preferred
- Traffic encryption: IPSec or MACsec on physical path; TLS on application layer

---

### Pattern 2: SD-WAN Branch to AI Services

For branch office users accessing AI services through the enterprise SD-WAN fabric:

```
Branch Office User
  → SD-WAN Edge Device
  → SD-WAN Fabric (application-aware routing)
  → Enterprise Hub / Data Center
  → AI Gateway (AI-SVC-DMZ zone)
  → AI Processing Zone
```

**SD-WAN policy requirements:**
- AI traffic classified as enterprise application category
- AI Gateway FQDN registered in SD-WAN application catalog
- Traffic steered to enterprise hub — not direct internet breakout
- QoS policy: AI traffic prioritized below clinical/voice but above bulk transfer
- Branch-to-AI-data-zone direct connectivity: never permitted

**SASE integration:**
- AI Gateway endpoints registered in SASE policy engine
- Device posture check enforced at SD-WAN edge before AI session establishment
- CASB policy applied to any SaaS-delivered AI components

---

### Pattern 3: Governed External LLM API Connectivity

When LLM inference is provided by a governed external API (not privately hosted):

```
AI Processing Zone (RAG Engine)
  → Egress Proxy (controlled outbound path)
  → Internet (TLS encrypted)
  → Approved LLM Provider Endpoint (FQDN allowlist)
```

**Controls required:**
- Egress proxy enforces FQDN allowlist — only approved LLM endpoint FQDNs permitted
- No other outbound internet access from AI Processing Zone
- All egress traffic logged with FQDN, bytes transferred, and response code
- Vendor data handling agreement executed before any traffic flows
- Zero-data-retention API configuration confirmed with vendor
- Mutual TLS preferred where vendor supports it

**FQDN allowlist example (synthetic):**
```
api.approved-llm-provider.com
auth.approved-llm-provider.com
```

All other outbound destinations from AI Processing Zone: DENY + LOG

---

### Pattern 4: Multi-Site Replication (High Availability)

For organizations requiring geographic redundancy of AI infrastructure:

```
Primary Site (AI Infrastructure)
  ↔ Private WAN / MPLS / SD-WAN
  ↔ Secondary Site (AI Infrastructure — standby or active-active)
```

**Replication requirements:**
- Vector database replication: async replication to secondary site
- Configuration synchronization: real-time via configuration management system
- Failover target: RTO ≤ 4 hours for full AI service restoration
- DNS failover: AI Gateway FQDN fails over to secondary site endpoint
- Replication traffic encrypted in transit

---

## Network Performance Requirements

| Path | Latency Target | Bandwidth Minimum | Redundancy |
|---|---|---|---|
| User → AI Gateway | < 50ms (on-premise) | 10 Mbps per 100 concurrent users | Dual path preferred |
| AI Gateway → RAG Engine | < 5ms | 1 Gbps | Redundant links |
| RAG Engine → Vector DB | < 2ms | 10 Gbps | Redundant links |
| AI Processing → LLM Endpoint | < 100ms (external API) | 100 Mbps | Dual ISP egress |
| Branch → AI Gateway (SD-WAN) | < 150ms | Per SD-WAN QoS policy | SD-WAN path diversity |

---

## Connectivity Security Requirements

| Requirement | Implementation |
|---|---|
| All AI traffic encrypted in transit | TLS 1.2 minimum; TLS 1.3 preferred |
| Private circuit for cloud connectivity | Dedicated interconnect — no public internet path for AI data |
| Egress proxy for external LLM API | FQDN allowlist enforced; all egress logged |
| No AI zone to internet direct access | Firewall deny-all on AI zone internet egress except via proxy |
| Branch AI access via SD-WAN only | No direct branch-to-AI-data-zone connectivity |
| All connectivity changes via change control | Network change requires Architecture and Security review |

---

## Monitoring and Alerting

| Metric | Alert Threshold | Owner |
|---|---|---|
| Private circuit utilization | > 80% sustained | Network Operations |
| Circuit availability | < 99.9% monthly | Network Operations |
| AI Gateway to RAG Engine latency | > 10ms P95 | AI Operations |
| Egress proxy blocked requests | Any unexpected FQDN block | Information Security |
| SD-WAN path quality degradation | MOS score drop or packet loss > 1% | Network Operations |

---

*All content is synthetic. No real network configurations, IP addresses, or vendor names are included.*