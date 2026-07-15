---
name: SLA Breach Risk Tiering
description: Tier every open ticket by SLA exposure — Breached / Critical / High / Watch — based on remaining time to the SLA target, adjusted by escalating and mitigating factors, with a recommended move per tier.
category: QA & Closure
tools: [search_tickets, list_boards, list_ticket_priorities, add_ticket_note, update_ticket]
---

# SLA Breach Risk Tiering

Turns "what's about to breach?" into a four-tier board: already breached, breaching within hours, breaching today/tomorrow, and worth watching. Remaining time sets the base tier; what the thread shows (angry VIP vs. fix pending confirmation) moves tickets up or down.

## When to use

- "What's at risk of breaching?" / "tier the queue by SLA risk" / "anything breached overnight?"
- The dispatcher's morning pass and pre-EOD pass.
- Feeding an escalation huddle a defensible priority order.

## Steps

1. Establish the SLA scheme: per-priority response/resolution targets from the desk's configuration as the user states them, or the priority mapping via `list_ticket_priorities` with the desk's default targets. If no targets are known, ask — never invent an SLA.
2. Pull open tickets with `search_tickets`, split per board (`list_boards`) and per priority; disclose result caps.
3. For each ticket, compute remaining time to the nearest applicable SLA target (first response for unanswered tickets, resolution otherwise), respecting SLA clock pauses where waiting statuses legitimately pause the clock — but verify the wait is real (a client-visible ask exists), not just a parked status.
4. Assign the base tier by remaining time:
   - **Breached** — target already passed.
   - **Critical** — breaches within the current shift (default < 4 business hours).
   - **High** — breaches within the next business day.
   - **Watch** — breaches within the tier horizon (default 3 business days).
5. Adjust one tier up or down for factors read from the thread, and say which factor moved it:
   - Escalating: visible client frustration or executive contact on the thread, repeat issue for this client, ticket unassigned, prior breach on the same ticket, "the VIP board" or contractual client flags the user has defined.
   - Mitigating: fix delivered and pending client confirmation, client agreed to a new timeline in writing, scheduled appointment before the target.
   Breached never adjusts down — a breach is a fact, not a judgment.
6. Recommend one move per ticket: assign now, escalate to <role>, respond now (first-response saves are the cheapest), request an SLA pause with documented client agreement, or accept-and-communicate for unavoidable breaches.
7. Output the tier board: Breached first with age-past-target, then Critical / High / Watch each sorted by remaining time; per line: ticket, client, priority, remaining (or overdue) time, adjustment factor if any, recommended move. On request, `add_ticket_note` a plain-text risk flag on specific tickets or `update_ticket` to assign/escalate — with confirmation, never in bulk.

## Guardrails

- Never invent SLA targets; if the scheme is unknown or a ticket's target is ambiguous, put it in an UNKNOWN bucket and say what is missing.
- Time math is stated in business hours with the assumption shown; when the desk's business calendar is unknown, say so.
- Factor adjustments must cite thread evidence ("client escalated to owner in note of <date>") — no vibes-based tier bumps.
- Breached tickets are reported without spin: age past target, not "slightly over."
- Read-mostly: notes, assignments, and escalations happen only on explicit approval.
- Disclose result caps — a missed board in an SLA sweep is exactly the ticket that breaches.
