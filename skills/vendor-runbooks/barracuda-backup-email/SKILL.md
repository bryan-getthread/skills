---
name: Barracuda Backup & Email Alerts
description: A Barracuda alert landed from one of two product lines — Backup (job/appliance alerts) or Email Protection / Email Gateway Defense (held mail, impersonation, ATP) — route to the right runbook and don't let a security block masquerade as a delivery complaint. Verify against Barracuda's current documentation.
category: Vendor Runbooks
tools: [search_tickets, search_clients, search_contacts, search_itglue, add_ticket_note, update_ticket]
---

# Barracuda Backup & Email Alerts

Barracuda spans two unrelated MSP-facing lines that arrive on the same desk under the same brand, and the first job is telling them apart: **Barracuda Backup** (appliance/cloud backup jobs) and **Barracuda Email Protection / Email Gateway Defense** (secure email gateway — quarantine/held mail, impersonation and ATP detections). This skill routes each to its owning canon; it does not replace them. Barracuda's product names and consoles change (Essentials → Email Gateway Defense) — verify against Barracuda's current documentation.

## When to use

- A Barracuda Backup job failure or appliance-health alert lands as a ticket
- A Barracuda Email Gateway "message held / quarantined" notice or release request arrives
- A Barracuda impersonation, ATP/sandbox, or account-takeover detection lands

## Steps

1. Identify the line first from the alert source and language — Backup vs Email. Never assume from "Barracuda" alone; the two have opposite response postures (a held email and a failed backup share nothing but the vendor).
2. Backup line → follow backup-failure-triage:
   - Classify the failure family: source unreachable at schedule, destination/cloud-replication capacity, credential/data-source auth (Exchange/SQL/VMware/Hyper-V/M365), or appliance-health (disk, temperature, replication lag on the physical/virtual appliance).
   - Recurrence via search_tickets (same device, same class, 30–90 days); one-off with a later success is noise, repeated same-class is a problem ticket.
   - Verify recoverability by last successful backup per source; never claim data is safe. Barracuda Backup replicates to Barracuda Cloud or a second appliance — state which copy is affected and its last good date.
   - Handle-here vs escalate to Barracuda support (repeated failures after remediation, integrity errors, or multi-client simultaneous failure → check status first).
3. Email line → this is security, not just deliverability:
   - Held/quarantined message release → run quarantine-release-request. Verify the recipient's authorization and inspect the message itself before any release; a user insisting "just release it" from a possibly-compromised mailbox is a known pattern. Bulk/spam differs from a message held for a threat verdict — the latter is not a release-on-request.
   - Impersonation / BEC detection (display-name spoof, lookalike domain, wire/gift-card lures) → run vendor-fraud-bec-alert / phishing-triage; treat as a live social-engineering attempt, check for sibling deliveries to other users, and never release an impersonation-flagged message to satisfy urgency.
   - ATP/sandbox verdict on an attachment/link → treat per phishing-triage; a "clean after detonation" verdict still warrants checking who received it and clicked.
   - Account-takeover signals from Barracuda's inbound/outbound detection → branch to compromised-account-containment, identity first.
4. Route to the correct client per security-alert-response (email line) / backup-failure-triage (backup line) using tenant/appliance/recipient fields, not brand or name similarity. Low confidence → flag, don't reassign.
5. Document the decision, not just the action: which line, the classification/verdict, containment or recoverability state, and — for email-security events — classify per soc-classification-tree. Client-facing wording per defensive-writing-standard.

## Guardrails

- Decide the line before doing anything; the two Barracuda products demand opposite postures.
- Backup: never claim data is safe — last successful backup per source, stating cloud vs appliance copy; don't clear the alert (it's evidence).
- Email: a message held on a threat verdict is not a release-on-request — verify authorization, inspect the message, and never release an impersonation/ATP-flagged item to satisfy urgency.
- Identity first on any account-takeover signal; a release request from a suspect mailbox is not authorization.
- Console/gateway actions (releases, allow/block entries, restores) are technician steps the agent directs and records; the agent has no Barracuda console access.
- Allow/block entries added while working are policy decisions — narrowest scope, named approver, review date; never allow a security-verdict sender to end a complaint.
- Degradation: without documentation access (search_itglue), the client's Barracuda scope (which line, which tenants/appliances) may be unclear — say so rather than assuming.
