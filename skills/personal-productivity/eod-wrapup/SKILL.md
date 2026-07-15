---
name: End-of-Day Wrap-Up
description: A technician closing out their day — sweep my tickets for anything stale >24h, awaiting-client without a follow-up sent, status mismatches, and line up tomorrow's first moves.
category: Personal Productivity
tools: [search_tickets, add_ticket_note, update_ticket]
---

# End-of-Day Wrap-Up

The hygiene sweep before you log off: catches the things that quietly rot overnight — unanswered clients, wrong statuses, missing time — and hands tomorrow-you a ready first move.

## When to use

- "End of day wrap-up" / "close out my day"
- "Anything I forgot to follow up on before I leave?"
- End-of-shift routine (also a natural scheduled-Flow candidate)
- Friday variant: "wrap up my week"

## Steps

1. Pull the requesting member's open tickets (search_tickets). Note any result cap in the output.
2. Run four hygiene checks and report each as its own section (skip a section entirely if it's clean — say "clean" in one word, don't pad):
   - **Stale >24h:** tickets with no activity from you in over 24 hours that are still yours to move. For each: ticket, days quiet, suggested action.
   - **Awaiting client, no follow-up sent:** tickets in a waiting-on-client state where the last outbound message predates the wait — the client may not even know you're waiting on them. Suggest a one-line follow-up for each.
   - **Status mismatches:** the thread says one thing, the status says another (client replied but the ticket still shows waiting-on-client; work clearly finished but not closed; ticket says in-progress with no activity for days). State what the status should probably be.
   - **Missing wrap-up:** tickets you worked today with no note or time entry recorded — tomorrow-you won't remember the details; tonight-you still does.
3. For each finding, propose the specific fix (the follow-up text, the corrected status, the note to add). Present all proposed fixes as a batch and ask once: "Apply these?"
4. Only after explicit confirmation, apply approved fixes: add_ticket_note for follow-ups and wrap-up notes, update_ticket for status corrections. Keep notes destined for a PSA sync plain text — no markdown, no emojis.
5. Finish with **Tomorrow's first moves:** the 2–3 tickets to open first tomorrow and why (who's been waiting longest, what's scheduled, what breaches first). One line each.

## Guardrails

- Confirm before every write — the sweep proposes, the member disposes. Never auto-send client-facing follow-ups.
- Scope strictly to the requesting member's own tickets.
- A status suggestion is a suggestion: if the thread is ambiguous about the true state, flag it as "check this one" rather than proposing a change.
- Never fabricate a wrap-up note for work you can't see evidence of — prompt the member for what they did instead of inventing it.
- Zero-assumption rule on notes: record what was done, never convert a plan or recommendation into a completed action.
- Keep the whole wrap-up skimmable; the member is trying to leave.
