---
name: VIP Priority Handling
description: Detect VIP contacts and VIP clients at ticket intake, apply the configured priority bump, and fire the desk's notify rules — without letting VIP status distort classification.
category: Triage & Routing
tools: [search_tickets, search_contacts, search_clients, update_ticket, add_ticket_note, list_ticket_priorities]
---

# VIP Priority Handling

Some contacts and clients get white-glove treatment by agreement. This skill spots them at intake, bumps priority per the configured VIP policy, and posts the notify-rule note so the right people hear about it.

## When to use

- New tickets should be checked for VIP requesters as part of intake.
- A flow fires on ticket creation to apply VIP handling automatically.
- "Is this person a VIP, and did we handle the ticket accordingly?"

## Steps

1. Read the ticket with `search_tickets`: requester, client, current priority, and the issue itself.
2. Check VIP status two ways: `search_contacts` for a VIP flag/type on the contact record, and `search_clients` for a VIP or white-glove tier on the company. Treat only explicit flags/tiers as VIP — job titles alone (e.g. "CEO" in a signature) are a lead to verify, not proof.
3. If VIP: apply the configured bump — typically raise priority one level (or to the VIP floor level) via `update_ticket`, using levels from `list_ticket_priorities`. Never lower an already-higher priority.
4. Fire the notify rules as configured: post a plain-text internal note tagging the ticket as VIP-handled and stating who per policy should be notified (account manager, dispatcher, on-call lead). If the desk uses a VIP board or owner routing, apply that too.
5. Report: VIP source (contact flag vs client tier), what changed, and which notify rule fired. If not VIP, say so plainly and change nothing.

## Guardrails

- VIP status must come from a record flag or configured list — never inferred from tone, title, or email domain prestige. Unverifiable "I'm important" claims get normal handling plus a flag for the account manager.
- VIP affects priority and notification only; it must not distort classification, severity facts, or duplicate handling.
- Never downgrade: if the ticket already sits above the VIP floor, leave priority alone.
- The bump is bounded by the configured policy — VIP does not automatically mean the top priority reserved for outages.
- Notes are plain text; do not put "VIP" language in any client-facing reply (it is internal handling, and other clients' tickets must never reference tiers).
- One bump per ticket, ever: if a VIP-handling note from this skill already exists, stop.
