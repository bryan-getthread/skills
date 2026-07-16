---
name: CSM Quarterly Ritual
description: A CSM's quarter-boundary runbook — QBR/SBR prep per client, Liongard QBR evidence packs, and IT-roadmap refresh — run when a CSM says "QBR season", "prep my quarterly reviews", or "quarterly account cycle".
category: Role Rituals
tools: [search_tickets, search_clients, liongard_environment, liongard_metric]
---

# CSM Quarterly Ritual

QBR season as a runbook instead of a scramble: for every client due a business review this quarter, build the review, back it with real environment evidence, and leave the roadmap fresher than you found it.

## When to use

- "It's QBR season — get me organized" / "prep my quarterly reviews."
- Quarter boundary, planning the coming quarter's review calendar.
- A new CSM inheriting a book and needing the quarterly machine started.

## Steps

1. **Slate the quarter (20 min)** — list clients due a QBR/SBR this quarter with dates (booked or to-book). Order by date; clients with open risk flags from `skills/role-rituals/csm-monthly-ritual` go earliest.
2. **Per client, in date order:**
   a. **Review prep** — run `skills/account-management/qbr-and-sbr-prep`: the deck-ready narrative of the quarter (tickets, trends, wins, issues, recommendations).
   b. **Evidence pack** — run `skills/liongard-inspectors/liongard-qbr-evidence-pack` where the client has Liongard coverage: environment facts, posture changes, before/after proof. State dataprint age; degrade to docs/ticket-history where the inspector is absent.
   c. **Roadmap refresh** — run `skills/account-management/it-roadmap-builder` in refresh mode: mark last quarter's roadmap items done/slipped/dropped, and add what the quarter's evidence justifies. The QBR presents a living roadmap, not a new one each time.
3. **Track the slate** — keep a simple table (client, date, prep / evidence / roadmap status) and re-run this ritual weekly during QBR season until the slate is done.
4. Output per run: the slate table with status, plus links/locations of each finished prep pack where the CSM's materials live.

## Time-short version

For the single nearest QBR only: step 2a (prep) and 2c (roadmap refresh). The evidence pack degrades to the top 3 environment facts; note the shortcut in the pack.

## Guardrails

- Never present stale evidence: every Liongard-sourced fact carries its dataprint age, and anything older than the quarter gets re-verified or cut.
- Roadmap refresh is honest about slippage — items quietly deleted instead of marked slipped will resurface in the client's memory at the worst time.
- Recommendations in the QBR must trace to this quarter's evidence; no boilerplate upsell slides.
- All materials are internal drafts until the CSM reviews — nothing is sent or shared with the client by this ritual.
- Skip-with-note beats fake completion: a client skipped this quarter is listed as skipped with a reason, not omitted from the slate.
