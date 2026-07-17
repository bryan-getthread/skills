---
name: Chat-to-Ticket Conversion
description: A live Messenger chat turned into a real issue — capture the chat's full context into a proper ticket (title, description, contact, priority, steps already tried) so the user never has to repeat themselves. Use when a chat outgrows quick-answer territory.
category: Voice & Messenger
tools: [search_tickets, update_ticket, create_ticket, assign_contact, add_ticket_note, log_time_entry, list_ticket_priorities]
connectors: []
---

# Chat-to-Ticket Conversion

**When to use:** "This chat is a real issue — make it a proper ticket" / a Messenger conversation reveals a problem needing escalation, scheduling, or another team / the chat's quick fix failed and the issue needs to persist past the session / end-of-chat hygiene before handing to the queue.

## Prompt

```
The moment a chat stops being a quick question and becomes real work, everything said so far
must become ticket structure — issue, environment details, steps already tried — so the next
tech starts where the chat left off, not from zero.

1. Read the entire chat thread via search_tickets. In Thread, a Messenger chat typically
   already lives on a ticket — the job is usually STRUCTURING that ticket, not creating a
   second one. Create a separate ticket (create_ticket) only when the chat contains a
   genuinely distinct second issue or desk process requires a different board.

2. Extract from the chat, quoting the user where load-bearing:
   a. The issue as the user described it, plus error messages, device/app names, and when it
      started.
   b. Environment facts surfaced mid-chat (OS, location, "it works on my laptop but not the
      desktop").
   c. STEPS ALREADY TRIED — both what the tech suggested and what the user did, with
      outcomes; this is the part whose loss forces the user to repeat themselves.
   d. Any commitments made in chat ("we'll schedule a tech", "I'll follow up tomorrow") and
      stated urgency/deadlines.

3. Structure the ticket with update_ticket: a searchable title ("<symptom> — <system>", not
   "chat with <user>"), the extracted summary as description, priority via
   list_ticket_priorities from stated impact, correct contact via assign_contact if the chat
   identified someone other than the current record.

4. Post the structured summary as a plain-text note (add_ticket_note): issue, environment,
   steps tried with outcomes, current state, next action + owner, commitments. This note is
   the handoff artifact.

5. Offer log_time_entry for the chat time — chat work is the most under-logged time on most
   desks.

6. Tell the user in-thread before the chat ends (per Live Chat Etiquette): ticket number,
   what happens next, and that they won't need to re-explain.

7. Output: ticket number, the structured summary, and anything the chat left unresolved or
   ambiguous.

Guardrails: the no-repeat guarantee is the point — if a detail was said in chat, it must be
findable on the ticket; when summarizing, prefer including a marginal detail over dropping
it. Quote, don't diagnose: the description records what the user reported; hypotheses go in
the internal note, clearly labeled. Never mark steps as "tried" unless the chat shows they
were completed. Chats capture credentials disturbingly often; never copy passwords/MFA codes
into fields or notes — "credential shared in chat, advised rotation" is the correct trace.
Duplicate check before any create_ticket: if an open ticket already covers this issue for
this user, structure and cross-reference instead. Plain-text notes for PSA-synced tickets;
keep the user-facing wrap-up in the chat's language.
```
