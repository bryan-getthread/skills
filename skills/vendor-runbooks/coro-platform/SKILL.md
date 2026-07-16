---
name: Coro Platform Alerts
description: A Coro modular-security alert landed — identify which module fired (endpoint, email, cloud apps, network, data governance, EDR), read Coro's severity and auto-remediation state, and work what the module didn't.
category: Vendor Runbooks
tools: [search_tickets, search_clients, search_contacts, search_itglue, add_ticket_note, update_ticket]
---

# Coro Platform Alerts

Vendor specialization of security-alert-response for Coro, the modular MSP-focused security platform. Coro's distinguishing trait is that one console fronts several independent modules (Endpoint Security/EDR, Email Security, Cloud Apps, Network, Data Governance, Endpoint Data Governance) — the first triage question is always *which module fired*, because that decides which generic runbook owns the investigation. The generic runbooks own the canon; this skill maps Coro's packaging onto them. Verify module names and remediation labels against Coro's current documentation — the module lineup evolves.

## When to use

- A Coro ticket/email arrives: a malware or suspicious-process endpoint detection, a phishing/email verdict, a cloud-app access anomaly, a network/DNS event, or a data-governance (PII/PCI exposure) finding
- A tech asks which Coro module raised an alert or whether Coro already remediated it
- A Coro auto-remediation fired and the desk must validate and follow through

## Steps

1. Identify the module first — Coro tags each ticket with the module. Endpoint/EDR → edr-detection-runbook. Email → phishing-triage / quarantine-release-request. Cloud Apps (M365/Google access anomaly, mass download, admin change) → the identity family (impossible-travel-runbook, compromised-account-containment). Network → dns-filtering / firewall investigation. Data Governance → dlp-alert-triage. Do not guess the module from the subject line; read the tag.
2. Parse the alert anatomy per security-vendor-generic's five questions (WHO/WHAT/WHEN/WHERE/ACTION-TAKEN), copying Coro's exact verdict wording. Route to the client per security-alert-response using tenant/workspace fields — Coro is multi-tenant; low routing confidence → flag for a human, no reassignment.
3. Read auto-remediation honestly: Coro auto-remediates many events (quarantine file, suspend user, block sender) depending on module policy. Treat every auto-action as claimed-but-verify — confirm by effect in the module console before trusting it. Detect-only / no action field → treat as live and contain first per the matching generic runbook.
4. Investigate per the module's generic runbook; branch on evidence (identity involvement → compromised-account-containment; ransomware behavior → ransomware-response). Consult Coro's documentation for field meanings (search_itglue for the client's stack docs) and mark inferred meanings as inferred.
5. Tune deliberately, never blanket-disable: a recurring benign detection on one module goes through security-noise-tuning as a scoped exception with an owner and review date.
6. Document module, five-question extraction, severity mapping, what Coro auto-did vs what the tech did, and classify per soc-classification-tree. Client-facing wording per defensive-writing-standard.

## Guardrails

- Never work a Coro ticket without first establishing which module fired — the module decides the runbook, and a mis-routed investigation misses the real scope.
- An auto-remediation is a claim until verified by effect in the module console; "Coro suspended the user" covers sign-in, not the attacker's persistence — sweep per compromised-account-containment before closing.
- Never close on Coro's severity label alone in either direction — evidence sets the tier.
- Exclusions/policy exceptions are security decisions: narrowest scope, named approver, review date.
- The agent has no Coro console access — quarantine release, user un-suspend, and policy changes are technician steps the agent directs and records.
- Degradation: without documentation access (search_itglue), which modules the client licenses may be unknown — say so rather than assuming full-platform coverage.
