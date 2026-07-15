---
name: Zapier PagerDuty On-Call
description: Page the on-call engineer for a P1 via PagerDuty, tell the requester who's on call and that they've been paged, and mirror the ack/resolve loop back to the ticket. Use for "page on-call", "who is on call right now", or wiring P1 paging into an after-hours flow.
category: Connectors
tools: [search_tickets, update_ticket, add_ticket_note, "zapier: PagerDuty"]
---

# Zapier PagerDuty On-Call

Close the after-hours gap: for a genuine P1, look up who's on call (`zapier: PagerDuty "Find User on Call"`), trigger the page (`zapier: PagerDuty "Trigger Event"`), tell the requester a named human is engaged, and keep the ticket in lockstep with the incident's ack/resolve state.

## When to use

- "Server down at <client> — page on-call."
- "Who's on call tonight?" (lookup only — no page)
- An after-hours flow needs a paging action for P1s that arrive outside coverage.

## Steps

1. **Qualify the P1** against the tenant's paging criteria (site down, server down, security incident, VIP-defined). Not qualifying → do not page; route to the normal queue and say why. Paging for non-P1s is how paging gets ignored.
2. **Dedupe before paging:** check the ticket and its recent siblings (`search_tickets`) for an existing page on the same incident. Already paged → add context to the existing incident rather than re-triggering. Use a deterministic dedup key (ticket number or alert correlation id) on the Trigger Event so PagerDuty folds repeats into one incident.
3. `zapier: PagerDuty "Find User on Call"` for the relevant escalation policy/schedule.
4. Confirm with the member (attended) or proceed per the flow's approved criteria (unattended), then `zapier: PagerDuty "Trigger Event"` with: client, one-line symptom, ticket reference, and callback path. The dedup key rides along.
5. Tell the requester: "<MSP>: we've paged our on-call engineer (<first name>) — you'll hear from us within <SLA window>." Name only what the tenant is comfortable sharing (first name/role); commit only to the SLA the desk actually has.
6. Mirror state to the ticket: `add_ticket_note` (plain text) for paged/acknowledged/resolved transitions as they're observed; `update_ticket` status per the desk's incident choreography. On resolve in PagerDuty, the *ticket* still closes through the desk's normal QA — PagerDuty resolution is not ticket resolution.

## Steps — ack/resolve loop

7. When following up: `zapier: PagerDuty "Find Incident"` (or the tenant's status source) → if unacknowledged past the escalation window, escalate per policy (next tier / manager) rather than silently waiting.

## Guardrails

- **Member-connected Zapier required** (tenant's PagerDuty account). Degrade to the tenant's manual on-call procedure (documented phone tree) — and say clearly that no page was sent.
- Page only on qualifying P1s; the criteria list comes from the tenant, not improvisation.
- The dedup check is mandatory — double-paging one incident at 3am costs goodwill the desk can't buy back.
- Never tell a requester someone was paged unless the Trigger call succeeded; verify the response, don't assume.
- Unattended paging flows follow Unattended Output Discipline: uncertain qualification = no page + queue for human triage.
- Task cost: each Zapier call = 2 tasks; a page cycle is typically 2–3 calls — don't poll incident state in a tight loop, check on the escalation window.
