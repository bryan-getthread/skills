---
name: Ticket Review Training
description: Turn a real resolved ticket into a sanitized teaching case — what was done well, what could improve, and the transferable lesson — for team training or self-review.
category: Training & Enablement
tools: [search_tickets, search_knowledge_base]
---

# Ticket Review Training

Converts one real resolved ticket into a teaching case: a sanitized narrative of what happened, a balanced review of what went well and what to improve, and the transferable lesson worth keeping. Useful for team meetings, mentoring, and self-review.

## When to use

- "Turn ticket <number> into a training example"
- "Review how I handled this ticket — what should I have done differently?"
- A lead wants a teaching case for the next team meeting ("find a good learning ticket from last week")
- Post-escalation learning ("what should L1 take away from this one?")

## Steps

1. Get the ticket. If given a number, fetch it; if asked to find one, search recent closed tickets for a case with teaching value (multi-step diagnosis, a recovery from a wrong turn, or an exemplary client interaction) and confirm the pick before writing it up.
2. Read the full thread: client messages, tech replies, internal notes, status changes, time entries, resolution.
3. Build the sanitized case narrative:
   - Setup: what arrived, how it was classified, placeholders for all names (<client>, <user>, <tech>)
   - Timeline: the key decision points in order — what was checked, what was concluded, where the turn happened
   - Outcome: the fix and how it was confirmed
4. Review against a balanced rubric — always both sides:
   - Done well: 2–4 specific moments (a sharp clarifying question, a good expectation-setting reply, clean notes) — quote the sanitized moment
   - Could improve: 2–4 specific moments (a skipped verification, a jargon-heavy reply, a silent 3-day gap, missing time entry) — each with what the better move was
5. Extract the transferable lesson: one paragraph stating the general principle this case teaches ("verify the symptom is reproducible before dispatching onsite"), phrased so it applies beyond this ticket.
6. If a relevant KB article exists (search_knowledge_base), link it; if the case reveals a documentation gap, flag that as a suggested KB addition — do not create it unprompted.
7. Output the case in a format ready to paste into a team meeting doc: Setup / Timeline / What went well / What to improve / The lesson. Offer a discussion-question variant (the same case with the review withheld, plus 3 questions for the team to debate).

## Guardrails

- Sanitize completely: no client, contact, or technician names, no credentials, no phone numbers or emails from the thread. If the reviewed tech is the requester themselves, "you" is fine.
- When the case is for group training, keep the handling tech anonymous — the point is the pattern, not the person. Never produce a group teaching case that shames an identifiable colleague.
- Be genuinely balanced: a case with nothing done well or nothing to improve is almost never true — look harder.
- Critique decisions against what was knowable at the time, not with hindsight ("given what the tech knew at step 2…").
- Quote and cite only what is actually in the ticket; do not invent dialogue, steps, or timestamps.
