---
name: Security Vendor Generic
description: A security alert arrived from a product with no dedicated vendor runbook — extract the alert anatomy, map its severity to the desk's tiers, separate containment-already-done from containment-needed, and build a vendor escalation package.
category: Vendor Runbooks
tools: [search_tickets, search_clients, search_contacts, search_itglue, add_ticket_note, update_ticket]
---

# Security Vendor Generic

The fallback framework for any security product not covered by a named vendor runbook. Every vendor's alert answers the same five questions if you interrogate it systematically; this skill is the interrogation. It layers on security-alert-response — which owns routing, tiering, and the investigation canon — and adds the vendor-agnostic extraction and escalation mechanics.

## When to use

- A security alert lands from a product with no dedicated skill in this category
- A new security vendor appears in a client's stack and the desk needs a working method today
- A tech asks "how do I even read this alert?"

## Steps

1. Extract the alert anatomy — force the alert to answer five questions, and record explicitly which ones it can't:
   - WHO: affected identity, device, or tenant. Which client (route per security-alert-response; never on name similarity).
   - WHAT: detection type and the vendor's verdict/confidence language. Copy the vendor's exact terms into the note — do not translate loosely.
   - WHEN: event time vs detection time vs alert time — the gaps between them are exposure windows.
   - WHERE: source and target (IP, host, mailbox, application).
   - ACTION TAKEN: what the product claims it already did (blocked, quarantined, isolated, disabled) vs detect-only.
2. Map severity: translate the vendor's scale onto the desk's tiers per security-alert-response — using the evidence, not the label. A vendor "critical" for a blocked commodity event can be Medium; a vendor "low" showing detect-only on a credential stealer is not. State the mapping and why.
3. Containment matrix — the core judgment:
   - Claimed-contained + verifiable → verify the claim (in the product console or by effect), then investigate scope calmly.
   - Claimed-contained + unverifiable → treat as uncontained until the technician confirms in the console.
   - Detect-only / allowed / no action field → treat as live; containment first (the matching generic runbook — edr-detection-runbook for endpoints, compromised-account-containment for identities — supplies the containment checklist).
4. Investigate per the matching generic runbook by event class (identity → impossible-travel-runbook family; endpoint → edr-detection-runbook; email → phishing-triage/quarantine-release-request; exposure feed → dark-web-alert-lifecycle). Identity first when the event is a login. Consult the vendor's own documentation for field meanings (search_itglue for the client's stack docs; web_search against the vendor's official docs where enabled) — and mark anything inferred as inferred.
5. When the product itself must answer (unexplained verdict, suspected product fault, remediation requiring vendor action) → build the vendor escalation package before contacting support: alert/incident ID and exact timestamps (with timezone), tenant/org identifier, product and agent versions if visible, the raw alert text, what was already checked and ruled out, actions taken so far with times, and the specific question being asked. One package, complete, in the ticket — support round-trips are where response time goes to die.
6. Document the decision, not just the action — the five-question extraction, severity mapping rationale, containment matrix outcome, and verdict — and classify per soc-classification-tree. Client-facing wording per defensive-writing-standard. If this vendor keeps appearing, flag that it deserves its own runbook.

## Guardrails

- Never trust an unverified containment claim — "the product says blocked" is a claim until confirmed in console or by effect.
- Never close on the vendor's severity label alone, in either direction — the evidence decides the tier and the verdict.
- Unfamiliar product ≠ lower standards: the canon holds — identity first, never close on assumption, contain fast on live threats, when in doubt escalate.
- Do not guess what vendor-specific fields mean; verify against the vendor's current documentation and say when a meaning is unconfirmed.
- Product-console actions are technician steps the agent directs and records; the agent has no security-console access.
- Exclusions/allowlists proposed during working are security decisions: narrowest scope, named approver, review date — regardless of vendor.
