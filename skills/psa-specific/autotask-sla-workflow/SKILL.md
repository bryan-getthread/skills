---
name: Autotask SLA Workflow
description: For desks synced to Autotask — read Autotask's SLA event model (First Response → Resolution Plan → Resolved), know exactly which statuses pause the clock, and assess breach risk correctly.
category: PSA-Specific
tools: [search_tickets, list_ticket_statuses, update_ticket, add_ticket_note]
---

# Autotask SLA Workflow

Applies to: **Autotask (Datto/Kaseya)**. Autotask SLAs are event-based: each ticket tracks separate targets for **First Response**, **Resolution Plan**, and **Resolved**, and only specific configured "waiting" statuses pause the clock. Misreading which event is at risk — or assuming a status pauses the clock when it doesn't — produces false confidence and real breaches.

## When to use

- "Is this ticket going to breach?" / breach-risk triage on an Autotask desk.
- Deciding whether moving a ticket to a waiting status actually stops the SLA clock.
- Explaining why a ticket breached despite "being responded to".

## Steps

1. Re-fetch the ticket with `search_tickets` at full detail. SLA state read from a stale listing is worthless — a first response may have landed in Autotask minutes ago.
2. Identify which SLA event is live for this ticket: First Response not yet met → the response clock runs; response met but no plan → Resolution Plan clock; plan met → Resolved clock. Each has its own target; report the *event*, not just "the SLA".
3. Check what satisfies each event on this desk. In Autotask these are configured per SLA, but the common pattern: First Response = a status change or outbound reply by a resource; Resolution Plan = a documented plan; Resolved = ticket completed. A silent internal note usually satisfies nothing — say so if a tech believes otherwise.
4. Check pause state: only the desk's configured waiting statuses (typically Waiting Customer, Waiting Vendor, and similar) pause the clock, and only for the events configured to pause. "In Progress with a note saying waiting on client" pauses nothing — the status itself must be set. Verify status names against `list_ticket_statuses`; never assume a status pauses by its name alone.
5. Assess risk: time remaining on the live event vs desk's working hours (Autotask SLAs usually count business hours — flag if you cannot confirm the coverage calendar). Tier per `skills/qa-and-closure/sla-breach-risk-tiering`.
6. Output: live SLA event, met/unmet events with timestamps where visible, pause state, breach risk tier, and the single action that satisfies the live event. Apply status changes with `update_ticket` only after confirmation, with a plain-text `add_ticket_note` when the change is clock-affecting.

## Guardrails

- **Sync lag:** always re-fetch before reporting breach risk or changing a clock-affecting status; an Autotask-side reply or status change may not have synced yet. Never declare a breach from Thread data alone — recommend confirming in Autotask for anything contractual.
- Never park a ticket in a waiting status *to stop the clock* without a genuine wait reason and a note stating what is being waited on — clock-gaming is an audit finding, not a technique.
- Do not assert pause behavior for a status you have not verified for this desk; SLA configs differ per tenant.
- Business-hours math: if you cannot see the coverage calendar, give the deadline as configured-hours-unknown and say so, rather than computing a wall-clock deadline that will be wrong.
- Do not invent SLA targets; if the ticket shows no SLA data, report "no SLA visible", not "no SLA".

## Cross-references

- Cross-PSA risk tiering: `skills/qa-and-closure/sla-breach-risk-tiering`
- Waiting-status hygiene: `skills/qa-and-closure/waiting-on-client-audit`
- Halo equivalent: `skills/psa-specific/halo-sla-and-priorities`
