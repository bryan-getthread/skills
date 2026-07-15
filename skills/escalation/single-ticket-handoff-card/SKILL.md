---
name: Single Ticket Handoff Card
description: Hand one ticket to another technician with a compact card — status table, watch points, overdue-task detection, and a one-sentence next action.
category: Escalation
tools: [search_tickets, add_ticket_note, update_ticket, search_members, schedule_ticket]
---

# Single Ticket Handoff Card

For handing exactly one ticket to a specific technician: a compact card that tells the receiver everything they need without reading the thread — current state at a glance, the traps, and the one thing to do next.

## When to use

- "Hand this ticket to <member>" / "reassign this to <member> with context."
- A technician is going on PTO and a specific in-flight ticket needs a new owner.
- A ticket is moving between teams mid-work (not a tier escalation — for that, use Escalation Prep).

## Steps

1. Read the full thread, notes, tasks, and schedule entries for the ticket via search_tickets.
2. Build the card:
   - **Status table** — one compact table: Status | Priority | Board | Client contact | Age | Last client contact | SLA state | Scheduled work.
   - **Story so far** — 2–3 sentences maximum: the issue and where the work stands.
   - **Watch points** — the traps: promised commitments and their deadlines, sensitive contact dynamics, VIP flags, things already tried that must not be repeated, dependencies on vendors or parts.
   - **Overdue check** — compare tasks, scheduled entries, and promised follow-ups against today's date; list anything already overdue at the moment of handoff, explicitly, so it transfers as a known debt rather than a surprise.
   - **Next action** — exactly one sentence, concrete and doable: "Call <user> after 2pm to confirm the replacement <device> arrived, then close." Not a list.
3. Show the card in chat. On confirmation: reassign via update_ticket, post the card as a plain-text internal note so it lives on the ticket, and offer to move any scheduled entries to the new owner via schedule_ticket.

## Guardrails

- One next action. If the ticket genuinely needs a plan, the next action is the first step of it — the rest goes under watch points.
- Overdue items must be surfaced, never silently rolled forward; the receiver decides how to recover them.
- Confirm the receiving technician (search_members) before reassigning; never guess between similarly named members.
- Do not invent commitments or deadlines — only report ones documented in the thread. Unknowns are stated as unknowns.
- The posted note is plain text (no tables in the note — flatten to labeled lines for PSA compatibility); the table form is for chat display.
