---
name: Noisy Asset & User Report
description: Someone asks which devices or users generate the most tickets for a client — the noisy-asset and frequent-flyer report with replace-or-train recommendations.
category: Reporting & Analytics
tools: [search_tickets, search_clients, search_contacts, list_boards]
---

# Noisy Asset & User Report

Rank the noisiest devices and users per client by ticket volume and time consumed, then recommend the fix per offender: replace or repair the asset, train or equip the user, or fix the environment around them.

## When to use

- "Which devices at <client> keep generating tickets?"
- "Who are the frequent flyers this quarter, and what do they need?"
- Feeding a QBR or hardware-refresh conversation with per-asset evidence.

## Steps

1. Confirm the scope (one client or all) and period (default: trailing 90 days — asset patterns need runway). Pull tickets with split searches per client per board; disclose caps.
2. Attribute tickets to assets and users from thread evidence: device names in titles/notes, contact on the ticket, asset references. Leave unattributable tickets out of the ranking and report the unattributed share.
3. Rank the top 5 noisy **assets** and top 5 noisy **users** per client by ticket count and estimated hours (logged time preferred; labeled estimates otherwise).
4. For each noisy asset, read the pattern and recommend accordingly:
   - Same hardware fault recurring or aging device → **replace/refresh** (note age if known).
   - Config or software issue recurring → **fix properly** (the RCA route).
   - Environmental (bad network drop, power) → **site fix**.
5. For each noisy user, diagnose before prescribing: recurring how-to questions → **training**; genuinely faulty equipment → the asset is the problem, not the user; role with heavy legitimate needs (a power user, a front-desk shared account) → **expectation/equipment adjustment**, not training.
6. Output per client: the two ranked tables, a one-line recommendation each, and estimated monthly hours recoverable. Methodology note: attribution basis, period, caps, unattributed share.

## Guardrails

- A high ticket count is a starting flag, never the verdict — a user who reports real issues diligently is an asset; distinguish "generates problems" from "reports problems" by reading what the tickets were.
- Frame user findings respectfully and factually; this report often reaches the client — no mockery, no "problem user" language, and consider whether user-level detail belongs in the client-facing copy at all.
- Attribution from thread text is approximate; publish the unattributed share and never force-match a ticket to a device on a name fragment.
- Exclude junk/NOC alert tickets from user noise (users don't generate those) and count them for assets only when a human actually worked them.
- Replace/train recommendations are advisory — cost and client-relationship context live with the account manager.
