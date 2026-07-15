---
name: RocketCyber SOC Triage
description: A RocketCyber SOC platform alert ticket arrived — parse the bracketed fields for client attribution, identify the alert class (firewall, Office 365, endpoint, dark web), and route to the matching specialist runbook.
category: Vendor Runbooks
tools: [search_tickets, search_clients, search_contacts, add_ticket_note, update_ticket]
---

# RocketCyber SOC Triage

Vendor specialization of security-alert-response for Kaseya's RocketCyber managed SOC platform. RocketCyber is an aggregator — its tickets wrap findings from many underlying "apps" (Office 365 monitoring, endpoint, firewall log analysis, dark web) — so the two vendor-specific jobs are: extract the client attribution from the structured subject/body fields, and route by alert class to the runbook that actually knows the domain. Verify field formats against RocketCyber's current output — integrations format subjects differently across versions.

## When to use

- A RocketCyber alert or incident ticket lands (often via PSA integration on a shared intake board)
- A batch of RocketCyber tickets needs client attribution and routing
- A tech asks "which client and which runbook is this RocketCyber alert for?"

## Steps

1. Parse the structured fields: RocketCyber tickets typically carry the customer/organization name and alert metadata in bracketed or delimited subject segments (e.g. "[<client>] <alert class> - <detail>") plus a body block with account/device/indicator fields. Extract: client/organization name, alert app/class, affected identity or device, severity, and timestamps. Treat the bracket-field client name as a claim to verify, not truth.
2. Client attribution per catchall-routing discipline: match the extracted organization against search_clients; corroborate with a second signal (UPN domain, device naming, contact match via search_contacts) before reassigning the ticket from the intake company. Never route on name similarity alone; low confidence → leave routing unchanged and flag for a human.
3. Route by alert class to the specialist runbook:
   - Office 365 / identity alerts (suspicious login, foreign login, inbox rule, admin change) → impossible-travel-runbook, inbox-rule-alert-runbook, or new-user-created-alert as fits.
   - Endpoint/EDR findings → edr-detection-runbook (or the vendor-specific EDR skill if the underlying product is known).
   - Firewall/syslog analysis alerts → assess per security-alert-response; port-scan and geo-block noise is a security-noise-tuning candidate after per-alert verification.
   - Dark web / breached credential → kaseya-darkweb-monitoring (same platform family) or dark-web-alert-lifecycle.
   - Anything unmapped → security-vendor-generic.
4. Severity mapping: translate RocketCyber's severity to the desk's tiers per security-alert-response and set the response clock accordingly — an aggregator's "critical" still gets the desk's own read of the evidence.
5. Prior-context check: search_tickets for the same client + alert class + account/device in 90 days; recurring benign findings are noise-tuning candidates, each occurrence still verified before closing.
6. Document attribution evidence (which fields matched what), the routing decision, and the runbook handoff in a plain-text note; classify per soc-classification-tree.

## Guardrails

- Bracket-field client names are attacker-influenceable and typo-prone — two-signal corroboration before reassignment, always.
- Aggregator alerts inherit the generic canon: never close on assumption, identity first for login-class alerts, when in doubt escalate.
- Do not collapse distinct alert classes for the same client into one ticket — merge only true companions per alert-storm-merge with exact references.
- If the underlying source app is unclear, say so — do not guess which product detected what; name what the tech should confirm in the RocketCyber console.
- The agent has no RocketCyber console access; remediation and incident closure there are technician actions the agent directs and records.
