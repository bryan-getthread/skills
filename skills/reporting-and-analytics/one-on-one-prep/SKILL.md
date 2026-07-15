---
name: One-on-One Prep
description: A manager is preparing for a 1:1, 30-day check-in, or coaching conversation with a technician and wants a candid brief on their recent work.
category: Reporting & Analytics
tools: [search_tickets, search_members]
---

# One-on-One Prep

Give a manager a concise, candid brief on one technician's recent work — load, speed, sentiment, wins, and improvement areas — sized to be read in the two minutes before the meeting.

## When to use

- "I have a 1:1 with <user> this afternoon — prep me."
- A 30-day or 90-day check-in on a newer engineer.
- "How is <user> doing lately?" ahead of a coaching conversation.

## Steps

1. Confirm the technician (search_members) and the period; default to the last 30 days.
2. Pull their tickets with search_tickets, splitting searches per signal (opened, closed, still open, negative sentiment) and per board where volume may hit a result cap.
3. Write **at most 300 words in five short paragraphs**:
   - **Load** — tickets worked, opened versus closed, and current aging/stalled items.
   - **Speed** — response and resolution patterns, distinguishing tech-side delays from client or vendor holds.
   - **Sentiment** — the overall trend across tickets with data, plus the single best and single worst interaction.
   - **Highlights** — exactly two standout pieces of work.
   - **Improvements** — exactly two specific, actionable areas to work on.
4. Aggregate rather than enumerate: report patterns and counts, not ticket-by-ticket listings.
5. Close with a one-line methodology note: period covered, which searches were run, and any result caps hit.

## Guardrails

- Manager's eyes only — write candidly and never share or post this to a ticket or client channel.
- Name a specific client or ticket only when it genuinely needs the manager's action; otherwise stay aggregate.
- Benchmark role-aware: a dispatcher, project engineer, or lead carries different open/closed expectations than an L1 — do not rank them against raw tech numbers.
- Never treat ticket volume alone as a performance signal; pair every count with an outcome (resolution, reopen, sentiment).
- Exclude junk/NOC boards and auto-closed statuses from all counts.
- If sentiment or timing data is missing for the period, say so rather than inferring.

## Consolidates

One-on-one prep, 1:1 meeting prep, 30-day ticket assessment, monthly engineer review brief, and the 21 mined manager-prep variants (≤300-word five-paragraph template family).
