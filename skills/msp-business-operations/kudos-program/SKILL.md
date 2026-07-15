---
name: Kudos Program
description: Stand up and run a peer-recognition program fed by ticket evidence — capture client praise as it lands, collect peer nominations, and publish a monthly roundup where recognition is specific, earned, and spread across roles. The program structure; praise-relay handles the individual moment.
category: MSP Business Operations
tools: [search_tickets, search_members, add_ticket_note, create_ticket]
---

# Kudos Program

Builds recognition into the desk's routine instead of leaving it to whoever happens to remember: client praise gets captured the day it arrives, peers can nominate each other with a reason, and once a month the roundup goes out — specific, evidence-backed, and deliberately inclusive of the roles clients never email about. Cross-references: praise-relay (communication) is the individual praise moment; weekly-wins-roundup (reporting-and-analytics) is the resolution highlights — this skill is the standing program around both.

## When to use

- "Set up a kudos/recognition program for the team."
- "It's month-end — pull together this month's kudos roundup."
- "A client just sent glowing feedback about <user> — log it for the program."
- Reviving a program that decayed into the same two names every month.

## Steps

1. Design (or load) the program shape with the owner — keep it light enough to survive:
   - **Inputs:** captured client praise, peer nominations (a sentence of what + why it mattered), and manager spot-notices.
   - **Cadence:** capture continuously, publish monthly.
   - **Format:** a short roundup in the team channel/meeting — names, what they did, why it mattered. No points, no leaderboard, no prizes ranked by count; the currency is being seen accurately.
2. Continuous capture:
   - When client praise lands, run the praise-relay moment (shout-out + client thank-you) and additionally log it for the program: a one-line entry on the program's tracking ticket (add_ticket_note) — who, what, source, date.
   - Peer nominations go to the same tracking ticket. Require the why ("covered my queue during the outage without being asked") — "great job" with no evidence doesn't teach anyone what great looks like.
3. Monthly assembly: sweep the period's inputs plus a light evidence pass of the month's tickets (search_tickets) for praise that arrived in threads and never got logged — client thank-yous buried in closures are the most under-captured signal on any desk.
4. Balance check before publishing — the step that keeps the program honest:
   - **Role coverage:** dispatchers, backline, and internal-ops folks don't get client emails; recognition for them comes from peer nominations and process evidence (the clean handoff, the schedule that held during flu season). If a month's list is all client-facing techs, go find what the quiet roles did.
   - **Repeat concentration:** the same star every month is probably accurate AND demotivating as the only story — keep their kudos and widen the frame, never manufacture balance by inventing praise.
5. Draft the roundup for the program owner to publish: each entry is name + the specific thing + why it mattered, in plain warm language. Two sentences per person beats a paragraph of adjectives.
6. Close the loop: log the published roundup on the tracking ticket, and let genuinely notable entries flow onward where the tenant wants them (a note the manager can pull at review time — recognition should compound).

## Guardrails

- **Recognition, never rating.** No counts-per-person totals, no leaderboards, no "most kudos" awards — the moment recognition becomes a metric, nominations become politics. Aggregate program stats (participation, role spread) are for the owner's program-health check only.
- Every kudo traces to something real: a ticket, a nomination with a why, a named moment. Never pad a thin month with invented or inflated praise — a short honest roundup beats a long hollow one.
- Absence of kudos is never evidence against anyone, never referenced in performance conversations, and never visible as a "zero" — the program only speaks in positives.
- Ask-first for the shy: some people genuinely hate public recognition; honor a standing preference for a private thank-you instead, no questions asked.
- Sanitize client praise for team-wide posting (client name only if the tenant is comfortable; never client-confidential context).
- This skill drafts and tracks; the program owner publishes. Nothing goes to team channels or clients autonomously.
