---
name: Tech Performance Review
description: A manager asks to evaluate a technician's performance over a period — closed, assigned, reopened, time logged, sentiment — with coaching angles, not a verdict.
category: Reporting & Analytics
tools: [search_tickets, search_members]
---

# Tech Performance Review

Build a per-technician period review from ticket evidence — throughput, reopen quality, time hygiene, and client sentiment — framed as coaching material for a human manager, never as a judgment on its own.

## When to use

- "Evaluate <user>'s performance over the last three months."
- "How many tickets did <user> close last month, and how many came back?"
- A manager gathering evidence before a performance or development conversation.

## Steps

1. Confirm the technician (search_members) and period; default to the last 90 days.
2. Run split searches per signal (and per board where volume warrants), disclosing any result caps:
   - Tickets **closed** by the tech in the period.
   - Tickets **assigned/worked** including ones still open.
   - Tickets **reopened** after their closure — then read the reopen cause and **exclude thank-you or courtesy replies** that merely bounced the status; count only genuine rework.
   - **Time logged** patterns: tickets worked with no time entries, or implausibly uniform entries.
   - **Sentiment** across their threads, with trend direction.
3. Compare against role-adjusted expectations: a dispatcher, PM, or escalation lead has structurally different numbers than an L1 — state which lens you used.
4. Present 2–3 strengths and 2–3 coaching angles, each tied to specific evidence patterns (aggregate; cite an individual ticket only as one representative example).
5. Close with a methodology note (period, searches, caps, exclusions) and an explicit line that this is input for the manager's judgment.

## Guardrails

- **Never render a PIP verdict, a fire/keep recommendation, or a fitness judgment.** Present evidence and coaching angles; recommend the manager apply human judgment and context you cannot see (PTO, project work, mentoring load).
- Never use ticket volume alone as a performance signal — always pair counts with quality signals (reopens, sentiment, aging).
- Exclude junk/NOC boards and auto-closed statuses from every count.
- Reopens caused by client thank-you replies are not rework — filter them explicitly and say you did.
- Manager's eyes only; do not post any of this to tickets or client-visible channels.
- If the data is thin (few tickets, no sentiment coverage), say the review is low-confidence rather than stretching conclusions.
