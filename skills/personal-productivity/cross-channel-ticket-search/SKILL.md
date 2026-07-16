---
name: Cross-Channel Ticket Search
description: Natural-language power search across ALL tickets — open and closed, every board and channel — filtered by tech, contact, client, age, or status, returned as a drill-down table. Answers the commonly requested "find me that ticket" workflow; runs manually on demand (Flows can't schedule it).
category: Personal Productivity
tools: [search_tickets, search_members, search_clients, search_contacts]
---

# Cross-Channel Ticket Search

One prompt, every ticket. Translate a plain-English question ("all closed printer tickets for <client> last month handled by <user>") into a scoped search across the whole desk — open and closed, every board and channel — and return a table the member can act on.

## When to use

- "Find every ticket where <contact> mentioned <keyword>"
- "Show me all closed tickets for <client> in the last 90 days"
- "What has <user> worked on this week across all boards?"
- "Search open and closed tickets for the outage on <date>"
- Any "I know we had a ticket about this once…" recall request

## Steps

1. Parse the request into explicit filters before searching: **who** (owner/member, contact, client/company), **what** (keyword, board, type, status), **when** (date range or age), and **scope** (open only, closed only, or both — default to both when the member says "all" or doesn't specify).
2. Resolve names to records first so the search is precise: search_members for a tech name, search_clients for a company, search_contacts for a person. If a name is ambiguous (multiple matches), list the candidates and ask which one rather than guessing.
3. Run search_tickets with the resolved filters. When the request spans multiple distinct signals (e.g. two different keywords, or two boards), run one search per signal and merge — a single over-broad query silently drops matches.
4. **Result-cap honesty:** if the number returned equals the tool's cap, say so explicitly ("showing the first 50 matches — narrow the date range or client to see the rest") rather than implying the list is complete. Never present a capped list as an exact count.
5. Return a drill-down table, one row per ticket: number, client (short), status, board/channel, owner, last-activity date, and a 5–8-word summary. Sort by the axis the member cares about (most recent first by default; by client or tech if they grouped that way).
6. Close with a one-line characterization ("14 matches — 3 still open, all on the Service board") and offer the natural next step ("Want the open ones as a digest, or drafts for the ones awaiting reply?").

## Guardrails

- Read-only. This skill finds tickets; it never changes status, owner, or notes. Any write is a separate, explicitly-requested follow-up.
- Resolve people and companies to records before filtering — a raw name string match misses tickets and pulls in the wrong client's data.
- Never fabricate ticket numbers, dates, or summaries. If a field is missing from the result, leave the cell blank rather than inventing a value.
- Respect the member's tenant scope; do not attempt to reach tickets outside the workspace the member belongs to.
- When both open and closed are in scope, label each row's status clearly so a closed ticket is never mistaken for actionable.
- This is an on-demand recall skill. It runs when a member asks — Thread Flows fire on ticket events only and cannot schedule or time-trigger it, so there is no unattended variant.
