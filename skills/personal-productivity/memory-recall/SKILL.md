---
name: Memory Recall
description: "I owe <user> something but don't remember what" — sweep my recent tickets, notes, and replies for open commitments I made and haven't delivered, to a person or across everything.
category: Personal Productivity
tools: [search_tickets, search_contacts, search_members]
---

# Memory Recall

The "I promised someone something" sweep: searches your own recent ticket activity for commitments you made — "I'll send that over", "I'll check with the vendor", "I'll call you Friday" — and shows which are still open, so the nagging feeling gets a ticket number.

## When to use

- "I owe <user> something but I can't remember what"
- "Did I promise anything to anyone this week that I haven't done?"
- "What did I say I'd do on <client>'s tickets?"
- Sunday-night dread, operationalized: "what's hanging over me?"

## Steps

1. Scope the sweep: a specific person (resolve via search_contacts — confirm which <user> if the name is ambiguous), a specific client, or everything. Default lookback: 14 days; widen on request.
2. Pull the member's tickets with their own recent activity in scope (search_tickets) and read their outbound messages and notes for commitment language: future-tense promises ("I'll…", "I will…", "let me…", "I'll get back to you by…"), committed deliverables (a quote, a document, a callback, an escalation), and datestamped promises ("by Friday", "tomorrow").
3. For each commitment found, determine whether it was visibly delivered afterwards in the same thread (the document sent, the follow-up posted, the status moved). Classify:
   - **Open:** promised, no visible delivery — this is the answer to "what do I owe".
   - **Probably done:** later activity plausibly fulfills it but not explicitly (say why it's only "probably").
   - **Delivered:** visibly closed out — mention only in the full-audit mode, not the quick ask.
4. Report open commitments ranked by overdue-ness: who, the exact promise (quote the sanitized line), the ticket, when it was made, any stated deadline, and days overdue. Lead with the single most overdue item.
5. If the sweep was for a specific person and nothing is found, say so plainly — and note the blind spots honestly: promises made outside tickets (calls not logged, hallway chats, direct email/Slack) are invisible to this sweep.
6. Offer next steps per open item: draft the delivery message now, or add an internal reminder note on the ticket — both only on confirmation.

## Guardrails

- Search only the requesting member's own activity; this is a personal memory aid, never a tool to audit a colleague's kept promises.
- Quote real lines from real tickets; never infer a commitment that isn't in the text ("they probably expected a follow-up" is not a promise).
- "Probably done" is never reported as done — uncertainty is the point of the tool; mislabeling it defeats it.
- State the coverage limits every time: lookback window, result caps if hit, and the out-of-ticket blind spot.
- Read-only by default; drafts and reminder notes only on explicit confirmation.
