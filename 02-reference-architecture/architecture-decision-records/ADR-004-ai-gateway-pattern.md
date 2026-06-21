# ADR-004: AI Gateway Pattern Selection

**Status:** Decided
**Date:** 2025
**Deciders:** Enterprise Architecture / Information Security
**Section:** 02 — Architecture Decision Records

---

## Context

The Secure Enterprise RAG Assistant requires a centralized control point for all AI requests — handling authentication validation, prompt inspection, rate limiting, DLP filtering, and request logging before any request reaches the RAG engine or LLM endpoint. This control point is the AI Gateway.

Three patterns were evaluated: a purpose-built AI gateway product, a traditional API gateway extended with AI-specific controls, and a custom-built gateway.

---

## Decision

**Selected pattern: Traditional API Gateway extended with AI-specific controls**

---

## Patterns Evaluated

### Pattern A: Purpose-Built AI Gateway Product
A dedicated AI gateway product designed specifically for LLM traffic management, including built-in prompt inspection, PII detection, and LLM-specific rate limiting.

**Pros:**
- AI-native features (prompt injection detection, token-based rate limiting, LLM routing)
- Faster time to capability
- Vendor-managed updates to injection detection rules

**Cons:**
- Emerging product category — vendor maturity varies
- Additional vendor relationship and contract
- May not integrate with existing enterprise API gateway infrastructure
- Potential data handling concerns with third-party prompt inspection

### Pattern B: Traditional API Gateway Extended with AI Controls (Selected)
The enterprise's existing API gateway platform extended with AI-specific middleware — custom prompt inspection, DLP integration, and token-based rate limiting.

**Pros:**
- Leverages existing enterprise API gateway infrastructure and operational knowledge
- No new vendor relationship required for gateway layer
- Full control over prompt inspection logic
- Integrates with existing gateway monitoring and logging
- Data never leaves enterprise-controlled gateway environment

**Cons:**
- Requires development of AI-specific middleware components
- Prompt inspection rules must be maintained internally
- Token-based rate limiting requires custom implementation

### Pattern C: Custom-Built Gateway
A fully custom gateway built specifically for this deployment.

**Pros:**
- Maximum flexibility and control

**Cons:**
- Highest development and maintenance burden
- Longest time to production
- No enterprise support tier
- Rejected — development cost and risk not justified

---

## Rationale for Pattern B Selection

**1. Operational continuity.**
The enterprise already operates an API gateway platform with established monitoring, alerting, change control, and operational runbooks. Extending this platform for AI traffic keeps AI gateway operations within an established operational model rather than introducing a new operational discipline.

**2. Data control.**
All prompt inspection, DLP filtering, and logging occurs within enterprise-controlled infrastructure. No request content is transmitted to a third-party gateway service for inspection.

**3. Vendor risk reduction.**
AI gateway products are an emerging category with varying levels of maturity and enterprise support. Extending an established enterprise platform reduces vendor concentration risk.

**4. Integration advantage.**
The existing API gateway integrates with the enterprise IdP, SIEM, and observability platform. These integrations are already established and operational.

---

## AI Gateway Capability Requirements

The extended API gateway must provide:

| Capability | Implementation Approach |
|---|---|
| Session token validation | IdP JWKS integration — validate token signature and claims |
| Prompt inspection | Custom middleware — regex + pattern matching; extensible rule set |
| Input DLP | DLP engine integration — pattern matching for sensitive data |
| Token-based rate limiting | Custom middleware — per-user token budget enforcement |
| Request routing | Standard API gateway routing to RAG engine endpoint |
| Structured request logging | Gateway logging middleware — JSON format to log aggregator |
| Output DLP | Custom response middleware — scan before delivery |

---

## Consequences

**Positive:**
- Leverages existing enterprise infrastructure and operational knowledge
- Full data control — no third-party prompt inspection
- Integrated with existing monitoring and SIEM
- Established change control and support processes apply

**Negative / Trade-offs:**
- AI-specific middleware requires development and maintenance
- Prompt injection detection rule set must be maintained internally
- May require gateway platform capacity increase for AI traffic volume

---

## Future Consideration

Purpose-built AI gateway products should be re-evaluated in 12–18 months as the market matures. If a product meets enterprise security, data handling, and operational requirements, migration from the extended traditional gateway may be appropriate. A new ADR will govern that decision.

---

*All content is synthetic.*