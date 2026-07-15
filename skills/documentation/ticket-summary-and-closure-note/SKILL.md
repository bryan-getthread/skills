---
name: Ticket Summary & Closure Note
description: Produce a clean summary of what happened on a ticket — resolution note, closure note, or a templated P1/P2 handoff summary — in the requested format and point of view.
category: Documentation
tools: [search_tickets, add_ticket_note]
---

# Ticket Summary & Closure Note

Summarize a ticket thread into the exact note the situation calls for: a resolution/closure
note, a "what was done" recap, or a templated escalation handoff summary.

## When to use

- "Write a closure note for this ticket" / "summarize what was done."
- "Give me a resolution note in our standard format."
- A P1/P2 is being handed to another tech or an after-hours team and needs a structured handoff summary.
- "Summarize this in first person for my time entry / PSA note."

## Steps

1. Read the full thread with `search_tickets`: replies, internal notes, and time entries. Establish problem → actions taken → outcome → anything outstanding.
2. Pick the format the request calls for:
   - **Resolution / closure note** — Problem, Actions Taken, Outcome, and (only if work remains) one clear Next Step with owner and deadline.
   - **P1/P2 handoff summary** — fixed template: Current Status; Impact (who/what is down); Timeline of key events with timestamps; Actions Taken So Far; What Has Been Ruled Out; Next Action + Owner; Client Communication State (who was told what, when).
   - **Plain recap** — short prose for a client-visible closure message.
3. Apply the requested point of view: default third person, or **first-person technician POV** ("I reset the profile and verified login") when asked — write as the assigned technician, not as an AI.
4. Apply formatting rules from the desk's **note-format-standard** skill if loaded. If the note will sync to a PSA, use **plain text only**: no markdown, no emojis, no smart quotes, no em-dashes.
5. Output the note in chat for review. Only post it with `add_ticket_note` when explicitly asked, and post handoff/QA summaries as **internal** notes unless told otherwise.

## Guardrails

- Do not invent detail the thread does not support — no fabricated confirmations, timestamps, or root causes. If the outcome is unclear, say "outcome not confirmed in thread" rather than asserting resolution.
- Follow the requested format precisely, including "no bullet points" or word limits when specified.
- First-person POV changes voice only — never add work the technician did not record.
- In handoff summaries, separate fact from hypothesis explicitly ("Confirmed:" vs "Suspected:").
- Never include credentials or one-time codes found in the thread in any summary.

## Consolidates

Closure-note requests, resolution summaries, P1/P2 handoff templates, and "next step" situational-summary skills.
