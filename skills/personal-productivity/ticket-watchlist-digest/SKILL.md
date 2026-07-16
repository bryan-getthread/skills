---
name: Ticket Watchlist Digest
description: Keep a personal watchlist of ticket IDs (in a note or Notion page) and, on demand, diff what's changed since the last run — new replies, status moves, closures — into a short digest. Answers the commonly requested "just track these specific tickets for me" workflow; runs manually on demand (Flows can't schedule it).
category: Personal Productivity
tools: [search_tickets, notion-fetch, notion-update-page]
---

# Ticket Watchlist Digest

Sometimes you don't want your whole queue — you want to keep an eye on a specific handful of tickets (an escalation you handed off, a client's open items, a project's threads). Keep those IDs in a personal list and get a short "what changed" digest whenever you ask.

## When to use

- "What's new on my watchlist?"
- "Anything change on the tickets I'm tracking?"
- "Add #<n> to my watchlist" / "check my watched tickets"
- Following a small set of tickets you don't own but care about

## Steps

1. Load the watchlist — the member's stored list of ticket IDs, plus the timestamp of the last digest. This lives wherever the member keeps it: a Thread note or a Notion page (notion-fetch, if they've connected Notion). If they're adding/removing IDs, update the stored list first (notion-update-page).
2. Pull the current state of each watched ticket with search_tickets.
3. **Diff against last run.** For each ticket, compare to the last-seen state and report only what changed since then: new client/tech replies, status moves, owner changes, closures. Tickets with no change get a single "no change" line at the bottom, not their own section.
4. Produce a short digest: one line per changed ticket — number, client (short), what changed, and whether it now needs the member. Lead with anything that needs them.
5. After presenting, update the stored watchlist's "last run" timestamp (and last-seen state, if tracked) so the next diff is accurate. Confirm before writing back to the member's note/Notion page.

## Guardrails

- Read-tickets-only: this digest never changes a ticket's status, owner, or notes — it only reports. The only writes are to the member's own watchlist store, and those are confirmed.
- If Notion isn't connected (and that's where the list lives), say so and fall back to a member-provided list in the prompt.
- Diff honestly — only report changes you can actually see since the last run; if there's no prior state to compare against, say "first run — here's current state" rather than inventing a diff.
- No fabrication of replies, status changes, or ticket numbers.
- Result-cap honesty if the watchlist is large enough to truncate.
- **Manual only.** Thread Flows have no schedule/cron, so this cadence skill runs on demand or from an external scheduler — never as a recurring flow.
