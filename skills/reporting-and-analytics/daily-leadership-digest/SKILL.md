---
name: Daily Leadership Digest
description: A service leader asks what needs their attention today — escalations, SLA breaches, at-risk clients, and staffing flags in one short daily view.
category: Reporting & Analytics
tools: [search_tickets, search_clients, search_members, list_boards]
---

# Daily Leadership Digest

The "what needs my attention" view for a service leader: only the items that warrant leadership eyes today — escalations, breaches, at-risk clients, staffing flags — everything else stays with the team.

## When to use

- "What needs my attention today?" from a service manager, director, or owner.
- A daily scheduled leadership briefing.
- "Anything on fire before I go into meetings?"

## Steps

1. Run split searches per signal (per board where needed; disclose caps), excluding junk/NOC boards and auto-closed statuses:
   - **Escalations** — open escalated or P1 tickets, and tickets whose thread shows an unfulfilled escalation request.
   - **SLA breaches** — breached or breaching-today tickets.
   - **At-risk clients** — clients showing 2+ risk signals: sentiment decline, multiple aging tickets, a recurring issue without RCA, or an unresolved P1. Never volume alone.
   - **Staffing flags** — a queue outgrowing its owner, unassigned urgent items pooling, or one tech visibly overloaded relative to role peers.
2. Filter hard: include only items that need a leadership decision, a client call, or a resource move. Items the team is already handling well get one summary line at most.
3. For each included item: what it is, why it clears the leadership bar, and the suggested move (call the client, reassign, approve overtime, chase the vendor).
4. Cap the digest at roughly 10 items; if more qualify, lead with the top 5 and summarize the rest by theme.
5. End with a one-line "green" note when a usual trouble area is quiet, plus a methodology note if any search capped.

## Guardrails

- Signal discipline is the product: an at-risk flag requires multiple corroborating signals — never flag a client or tech on volume alone.
- Staffing flags are observations with role/shift context, not performance judgments; route person-level concerns to the manager privately.
- Leadership-only distribution; do not post to tickets or client-visible channels.
- Do not fabricate risk where searches returned nothing — an honest "nothing needs you today" is a valid, valuable digest.
- Disclose result caps rather than presenting a partial scan as a full one.

## Running this unattended

> **Flows cannot schedule or time-trigger this.** Thread Flows fire on ticket *events* and conditions only — there is no schedule, cron, ticket-age, or elapsed-time trigger. This is a cadence/sweep skill, so run it **manually** on demand, or from an external scheduler that invokes Super Magic. A Flow can only reach it via **Run Skill** on a qualifying ticket event, never "every morning" or "after N hours". The output discipline below applies whenever it runs unattended.

- Entire reply is the digest, verbatim, fixed sections: Escalations / Breaches / At-Risk Clients / Staffing / All-Clear.
- Empty sections print "none". If every section is "none", the digest is one line: nothing needs leadership attention today.
