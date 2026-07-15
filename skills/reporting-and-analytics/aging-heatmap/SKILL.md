---
name: Aging Heatmap
description: Someone wants to see the open-ticket age distribution at a glance — a text heatmap of age buckets by board, client, or tech — plus the oldest open tickets with context, handled respectfully.
category: Reporting & Analytics
tools: [search_tickets, list_boards, search_clients, search_members, list_ticket_statuses]
---

# Aging Heatmap

Render the desk's open-ticket age distribution as a compact text heatmap — age buckets across, boards (or clients, or techs) down — so the backlog's shape is visible in one glance, then surface the oldest open tickets with enough context that the list reads as "here's what's stuck and why" rather than a wall of shame.

## When to use

- "Show me the aging picture" / "how old is our backlog?"
- A weekly backlog review or huddle wants the stuck-work view (pairs with Board Health Snapshot, which is one board deep; this is the whole-desk grid).
- "What are our oldest open tickets and why are they still open?"

## Steps

1. Confirm scope (default: all active boards via list_boards, excluding junk/NOC noise boards) and the row dimension — board (default), client, or tech. Identify which statuses count as "open" and, separately, which mean waiting-on-client (list_ticket_statuses).
2. Run split searches per row per age bucket (0–2d / 3–7d / 8–14d / 15–30d / 31–90d / 90d+). Split searches keep counts honest; record any caps.
3. Render the heatmap as a monospaced text grid: rows, bucket counts, row totals, and an intensity mark (e.g. · ▪ ▪▪ ▪▪▪) scaled within the report so hot cells jump out. Keep it narrow enough to read in chat.
4. Separate waiting-on-client tickets into their own column or a clearly-marked share per row — a 60-day ticket waiting on a client's vendor is a follow-up-cadence issue, not a neglected ticket, and lumping them poisons the whole read.
5. **Oldest tickets, respectfully** — list the 5–10 oldest genuinely-stalled open tickets with: age, client, one-line subject, last-activity age, and the visible reason it is stuck (waiting on parts, escalated upstream, scope dispute, genuinely dropped). The tone is "what does this ticket need to move" — never a tech's name in a blame frame; include the assignee only as "who to ask", and note when a ticket has outlived its original assignee.
6. Output: the heatmap, the waiting-on-client share, the oldest-tickets table, 2–3 patterns worth acting on (e.g., one board accumulating in 15–30d = a capacity or process pinch), and a methodology note — statuses counted as open, bucket edges, exclusions, searches run, caps hit.

## Guardrails

- Old tickets have reasons; the deliverable is unblocking, not shaming. Never frame the oldest-ticket list as a leaderboard of neglect, and never rank techs by aged-ticket count alone — queue mix and role drive it.
- Waiting-on-client and vendor-blocked tickets must be visibly separated from stalled internal work.
- Never present capped bucket counts as exact; disclose caps per cell if any search capped (a capped cell understates precisely the hot spots this report exists to find).
- Aggregate elsewhere, enumerate only in the oldest-tickets table — that is the one place specific tickets belong.
- Text grid only — no assumptions that the consumer can render images or wide tables; keep rows to what fits a chat window.

## Unattended (Flows) variant

- Your entire reply is the heatmap report, posted verbatim — no narration or questions.
- Fixed sections (Heatmap / Waiting-on-client / Oldest / Patterns / Methodology) in plain text, PSA-safe: no markdown tables or emojis if the destination syncs to a PSA note.
- If a bucket search fails, print "no data" for that cell rather than zero — zero is a claim.
