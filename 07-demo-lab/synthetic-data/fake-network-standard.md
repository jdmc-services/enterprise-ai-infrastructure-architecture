# SYNTHETIC DOCUMENT — FOR TESTING ONLY

**Document Title:** Enterprise Network Segmentation Standard
**Document ID:** DOC-SYNTH-002
**Version:** 2.3
**Classification:** Tier 2 — Internal
**Owner:** Network Architecture Team
**Approved By:** Enterprise Architecture Lead (Synthetic)
**Effective Date:** 2025-01-15
**Review Date:** 2026-01-15

> ⚠️ **THIS IS A SYNTHETIC DOCUMENT FOR PORTFOLIO AND TESTING PURPOSES ONLY.**
> It does not represent any real organization's network configuration. All content is fictional.

---

## 1. Purpose

This standard defines the network segmentation requirements for the enterprise environment, including VLAN architecture, zone isolation requirements, firewall policy standards, and change control procedures for network segmentation modifications.

## 2. VLAN Architecture Principles

2.1 All enterprise network segments must be assigned to a defined VLAN with a documented purpose, owner, and security classification.

2.2 VLANs must be assigned to security zones based on the sensitivity of the systems and data they carry. Zone assignment is permanent unless approved through the architecture change control process.

2.3 No VLAN may span multiple security zones. Inter-zone communication requires explicit firewall policy approval.

## 3. Security Zone Definitions

| Zone | Description | Trust Level |
|---|---|---|
| Clinical | Patient care systems, EHR, medical devices | Isolated |
| Enterprise User | Staff workstations and mobile devices | Untrusted |
| Enterprise Application | Business application servers | Semi-trusted |
| Infrastructure | Network infrastructure management | Restricted |
| AI Services | AI workload components | Dedicated — see AI Infrastructure Standard |
| DMZ | Internet-facing services | Untrusted |

## 4. Clinical Zone Isolation (Non-Negotiable)

4.1 The Clinical zone must have no direct network connectivity to any other zone except through explicitly approved, documented, and firewall-enforced paths.

4.2 Any connectivity to or from the Clinical zone requires Architecture Review Board approval and documented clinical justification.

4.3 Firewall deny rules for Clinical zone isolation are reviewed monthly and may not be modified without dual approval from Network Security and Clinical Technology leadership.

## 5. IoT Device Requirements

5.1 IoT medical devices must be placed in a dedicated IoT VLAN within the Clinical zone.

5.2 IoT VLANs must have deny-all default firewall policy with permit-by-exception for approved clinical application communication paths only.

5.3 IoT devices may not have direct internet access. All IoT internet communication must traverse an approved proxy.

## 6. Firewall Policy Standards

6.1 All firewall policies follow a deny-all default with permit-by-exception model.

6.2 Firewall rules must include: source zone, destination zone, protocol, port, purpose, owner, and approval date.

6.3 Firewall rules are reviewed quarterly. Rules without a documented owner or purpose are flagged for removal.

---

*SYNTHETIC DOCUMENT — NOT A REAL NETWORK STANDARD*