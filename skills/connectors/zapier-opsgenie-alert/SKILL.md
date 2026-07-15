---
name: Zapier Opsgenie Alert
description: Fire an Opsgenie alert when an SLA breach or major incident needs to reach the on-call rotation — Create Alert is the only Opsgenie action Zapier offers, so this skill does that one thing well and is honest about everything it can't do. Use for "page on-call via Opsgenie" or SLA-breach alerting in Opsgenie shops.
category: Connectors
tools: [search_tickets, add_ticket_note, update_ticket, "zapier: Opsgenie"]
---

# Zapier Opsgenie Alert

For desks whose on-call runs through Opsgenie: raise a well-formed alert — severity-honest, ticket-referenced, dedupe-keyed — via `zapier: Opsgenie "Create Alert"`. That is the entire Zapier surface for Opsgenie: **create only**. No acknowledge, no resolve, no who's-on-call lookup. This skill works within that honestly instead of pretending otherwise.

## When to use

- "Page on-call — the <client> outage qualifies as a major."
- An SLA-breach or major-incident flow needs to reach the Opsgenie rotation.
- PagerDuty shop instead → use Zapier PagerDuty On-Call, which has the richer surface (acknowledge/resolve/find-on-call).

## Steps

1. Qualify against the tenant's paging criteria: paging is for conditions that need a human *now*. An alert fired for a routine ticket erodes the rotation's trust in every future page. Unsure whether it qualifies → ask the member; don't page speculatively.
2. **Anti-dup check:** paging twice for one condition is noise with a pager attached. Check the ticket's notes for an alert already fired for this condition; found and nothing material has changed → report the outstanding alert instead of re-firing. Give the alert a stable identity by carrying the ticket number in the alert's message/alias so Opsgenie's own dedupe can also catch repeats.
3. Compose the alert to be actionable from the pager alone: message = "<severity>: <one-line issue> — ticket <number>"; description with client-safe impact, what's been tried, and where to engage (ticket link, swarm channel if one exists); priority mapped per the tenant's severity convention — never inflated to get faster pickup.
4. Show message + priority for confirmation (unattended flows: only fire on deterministic, pre-agreed conditions like a hard SLA breach), then `zapier: Opsgenie "Create Alert"`.
5. Record it: `add_ticket_note` (plain text) — alert fired, when, priority, and the alias/reference. This note is what step 2 reads next time.
6. **Be honest about the loop:** Zapier cannot see acknowledgement or close the alert. Whether on-call engaged shows up as a human in the ticket/channel — if nobody engages within the tenant's expected window, that's a fact to surface to the member for manual escalation (phone tree, backup contact), not a reason to fire more alerts. Closing the alert happens in Opsgenie by a human.

## Guardrails

- **Member-connected Zapier required** (Opsgenie). Degrade: fall back to the tenant's other urgent channels — Zapier Slack Escalation Ping, Twilio SMS, or the documented phone tree — and say Opsgenie wasn't reachable.
- Create Alert is the whole toolset: never claim an alert was acknowledged, escalated, or resolved — the skill cannot know. Report only "alert created".
- One condition, one alert; the ticket-note + alias dedupe check runs before every fire.
- Severity honesty is the rotation's currency: priority reflects the tenant's definitions, not urgency theater.
- Alert text reaches personal devices: client-safe identifiers, no credentials, no end-user personal data.
- Task cost: each Zapier call = 2 tasks — one create per genuine condition; the dedupe check is a ticket read, not an API probe.
