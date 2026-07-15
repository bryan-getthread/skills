---
name: Halo SLA and Priorities
description: For desks synced to HaloPSA — read Halo's SLA/priority interplay correctly: priority selects the response and fix targets within the client's SLA, and hold statuses pause the clock.
category: PSA-Specific
tools: [search_tickets, list_ticket_priorities, list_ticket_statuses, update_ticket, add_ticket_note]
---

# Halo SLA and Priorities

Applies to: **HaloPSA**. In Halo, the client's SLA and the ticket's priority work as a pair: the SLA defines a target matrix, and the priority row selects which **response target** and **fix (resolution) target** apply. Changing priority therefore changes the deadlines — it is an SLA action, not a cosmetic label.

## When to use

- Setting or changing priority on a Halo-synced ticket.
- "When is this due?" / breach-risk assessment on a Halo desk.
- Explaining why two same-priority tickets have different deadlines (different client SLAs).

## Steps

1. Re-fetch the ticket with `search_tickets` at full detail — priority, status, and SLA state may have moved Halo-side since your last read.
2. Read the pair: which SLA applies (from the client/contract) and which priority is set (`list_ticket_priorities` for the tenant's actual list). The deadlines are the intersection; never quote a target from priority alone without knowing the client's SLA, since Halo targets differ per SLA.
3. Distinguish the two clocks: **response target** (first agent response) and **fix target** (resolution). Report which is live and how much remains on each. A met response with a distant fix target is a different situation from an unmet response — say which.
4. Check pause state: hold-type statuses (waiting on customer/vendor/etc., per the desk's config) pause the SLA clock; verify the actual status against `list_ticket_statuses` rather than trusting a "waiting" phrase in a note. Only the status pauses the clock — a note does not.
5. When changing priority, treat it as a deadline change: state old → new priority and the resulting target shift, and justify from impact/urgency evidence in the thread (severity-forcing rules in `skills/triage-and-routing/ticket-triage` apply — security and multi-user impact force high priority). Apply with `update_ticket` after confirmation; note the reason via plain-text `add_ticket_note`.
6. Output: applicable SLA (as far as visible), priority, live clock(s) with remaining time or "target not visible from Thread", pause state, and breach-risk tier per `skills/qa-and-closure/sla-breach-risk-tiering`.

## Guardrails

- **Sync lag:** re-fetch before any deadline claim or priority change; a Halo-side response may have already met the target you are about to escalate over.
- Never lower a priority to avoid a breach — that is deadline-gaming and shows in Halo's SLA reporting; escalate instead.
- Never quote deadlines you cannot see: if SLA targets are not visible from Thread, report the priority and recommend confirming the target matrix in Halo rather than inventing hours.
- Business-hours calendars vary per SLA in Halo; do not convert targets to wall-clock times unless the coverage calendar is known.
- Priority changes on client-facing tickets may trigger Halo notifications — call it out before applying.

## Cross-references

- Autotask equivalent: `skills/psa-specific/autotask-sla-workflow`
- Clock-affecting transitions: `skills/psa-specific/halo-status-actions`
- Risk tiering: `skills/qa-and-closure/sla-breach-risk-tiering`
