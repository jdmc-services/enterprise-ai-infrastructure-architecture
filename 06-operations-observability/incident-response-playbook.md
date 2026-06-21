# Incident Response Playbook: Enterprise AI Infrastructure

**Section:** 06 — Operations and Observability
**Version:** 1.0
**Owner:** AI Operations / Information Security

---

## Purpose

This playbook defines the incident response procedures for the Secure Enterprise RAG Assistant. It covers classification, response actions, escalation paths, and post-incident requirements for the most likely incident scenarios.

---

## Incident Severity Classification

| Severity | Definition | Examples |
|---|---|---|
| P1 — Critical | Service unavailable or confirmed security breach | AI system down; confirmed prompt injection; clinical zone traffic; data exfiltration |
| P2 — High | Significant degradation or suspected security event | Elevated error rate > 10%; suspected injection; RBAC anomaly; auth spike |
| P3 — Medium | Minor degradation or informational security event | Latency degradation; cost anomaly; rate limit violations |
| P4 — Low | Informational; no service impact | Minor threshold breach; informational SIEM alert |

---

## Playbook 1: AI System Unavailable (P1)

**Trigger:** AI Gateway health check fails for > 2 minutes OR user reports confirm service unavailable.

```
IMMEDIATE (0–15 minutes):
1. AI Operations Lead acknowledges P1 alert
2. Confirm scope — is AI Gateway down or specific component?
3. Check component health dashboard for failure point
4. Attempt component restart per runbook SOP
5. Notify stakeholders: Enterprise Architect + affected user groups

SHORT-TERM (15–60 minutes):
6. If restart resolves: validate full system health check
7. If restart fails: escalate to vendor support (LLM or infrastructure vendor)
8. Assess failover to secondary site if available
9. Communicate estimated restoration time to stakeholders

RECOVERY:
10. Execute full health check sequence before declaring recovery
11. Monitor for 30 minutes post-recovery for stability
12. Document incident timeline in ITSM ticket
13. Schedule post-incident review within 48 hours
```

**RTO Target:** 4 hours

---

## Playbook 2: Confirmed Prompt Injection Attack (P1)

**Trigger:** SIEM alert AI_PROMPT_INJECTION confirmed by InfoSec review OR output validation detects system prompt echo.

```
IMMEDIATE (0–15 minutes):
1. InfoSec acknowledges P1 alert
2. Terminate offending session immediately
3. Preserve all logs for forensic review — do not alter or delete
4. Notify: CISO + Enterprise Architect + AI Operations Lead
5. Assess: Was injection successful? What response was returned?

SHORT-TERM (15–60 minutes):
6. If sensitive data was returned:
   - Notify Privacy Officer
   - Assess notification obligations
   - Document scope of exposure
7. Review SIEM for similar injection patterns from other sessions
8. Determine if control gap exists in prompt inspection engine
9. Update injection detection rules if new pattern identified
10. Consider temporary service suspension if systemic risk identified

RECOVERY:
11. Patch control gap before resuming service
12. Re-run injection test suite against patched controls
13. Document full incident timeline
14. Post-incident review within 48 hours
15. Update AI Risk Register if new risk variant identified
16. Update test case library with new attack pattern
```

---

## Playbook 3: Clinical Zone Traffic Detected (P1)

**Trigger:** SIEM alert AI_CLINICAL_ZONE_TRAFFIC — any traffic between AI zones and Clinical zone detected.

```
IMMEDIATE (0–15 minutes):
1. Network Security and CISO notified immediately — no delay
2. Identify traffic source and destination from firewall logs
3. Block offending traffic at firewall if not already blocked
4. Isolate affected AI component if compromise suspected
5. Notify Clinical Technology team — assess any clinical system impact

SHORT-TERM (15–60 minutes):
6. Forensic review of firewall logs — how did traffic traverse zone boundary?
7. Assess whether clinical systems were affected
8. Determine root cause: misconfiguration, compromise, or false positive
9. If compromise: initiate enterprise incident response process
10. If misconfiguration: remediate and validate before resuming AI service

RECOVERY:
11. Validate firewall deny rules are active and correct
12. Confirm clinical systems show no anomalous activity
13. Document full incident timeline
14. Post-incident review within 24 hours (elevated urgency)
15. Consider third-party forensic review for confirmed compromise
```

---

## Playbook 4: Unauthorized Retrieval Attempt (P1)

**Trigger:** SIEM alert AI_RBAC_FILTER_MISSING — retrieval event without RBAC filter applied.

```
IMMEDIATE (0–15 minutes):
1. InfoSec acknowledges P1 alert
2. Block session associated with unfiltered retrieval
3. Determine: was unfiltered retrieval a code defect or attack?
4. Review what documents were returned without filter
5. Notify Enterprise Architect and CISO

SHORT-TERM (15–60 minutes):
6. Code review of RBAC filter construction logic
7. Determine scope — was this isolated to one session or systemic?
8. If systemic: suspend AI service until filter is validated
9. Review all retrieval logs for period — identify any other unfiltered events
10. Remediate code defect or configuration issue

RECOVERY:
11. Deploy fix and validate RBAC filter is applied on 100% of queries
12. Run RBAC bypass test suite before resuming service
13. Document full incident timeline
14. Post-incident review within 48 hours
```

---

## Playbook 5: Cost Anomaly (P3)

**Trigger:** Daily token consumption > 120% of 7-day rolling average.

```
INVESTIGATION (within 2 hours):
1. AI Operations Lead reviews cost telemetry dashboard
2. Identify which user roles or query types drove anomaly
3. Determine if volume increase is legitimate (new use case, training) or anomalous
4. Check for rate limit violations or unusual query patterns

RESPONSE:
5. If legitimate volume increase: update baseline; review budget impact
6. If abuse pattern: review rate limiting configuration; adjust if needed
7. If system error causing token waste: identify and fix

DOCUMENTATION:
8. Log investigation findings in operations log
9. If budget impact is material: notify AI Governance Lead
```

---

## Post-Incident Requirements

All P1 and P2 incidents require:

| Requirement | Deadline |
|---|---|
| ITSM ticket opened | During incident |
| Incident timeline documented | Within 24 hours of resolution |
| Post-incident review meeting | Within 48 hours |
| Root cause documented | Within 5 business days |
| Control improvements implemented | Within 30 days (P1) / 60 days (P2) |
| AI Risk Register updated (if new risk) | Within 5 business days |
| Test case library updated (if new attack pattern) | Within 5 business days |

---

*All content is synthetic.*