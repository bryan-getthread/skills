---
name: Automation Completion Auto-Close
description: When note evidence shows an intent or external automation actually finished the work, verify it and close the ticket with an audit note — aborting the moment any human message arrives after completion. Answers the "auto-close tickets the automation already handled" workflow.
category: Automation & Flows
tools: [search_tickets, update_ticket, add_ticket_note, list_ticket_statuses]
connectors: []
---

# Automation Completion Auto-Close

**When to use:** An intent or automation resolves a request end-to-end (password reset, provisioning step) and the ticket lingers open; "close the ones the automation already finished"; a flow should sweep automation-handled tickets closed once the completion note lands. Fires on the NOTE-ADDED event (the completion note landing) — not a timer or "N minutes after".

## Prompt

```
Close tickets an automation genuinely finished — but only after confirming from note
evidence that it completed, and only if no human has said anything since. When in doubt,
leave the ticket open; a false close is worse than a lingering open ticket.

Your entire reply is the audit note, verbatim: either `AUTO-CLOSED. Automation: <name>.
Evidence: <note ref>. No human message after completion.` or `NO ACTION. Reason: <no
completion note | human message after completion | status missing>.`

1. Re-fetch the ticket with search_tickets and read the full thread, newest-first.

2. Find explicit completion evidence: a success message from the intent/automation, a
   "completed / done / provisioned" system note tied to THIS ticket's request — not a
   "started" or "queued" note, and not a client asking for the thing. No unambiguous
   completion note -> stop. Never fabricate completion or infer "it probably worked" from
   silence.

3. Abort on any human message after completion. If a technician or client posted anything
   (question, correction, new request) after the completion note, the ticket is live again
   -> do nothing. This abort is absolute.

4. Confirm the target closed/resolved status exists via list_ticket_statuses (respect the
   desk's closed-status taxonomy). Status missing on tenant -> no action.

5. Close with update_ticket to the resolved/closed status and post a plain-text audit note
   via add_ticket_note: which automation completed, the note it was evidenced by, and that
   no human message followed.

Status change and the audit note are the only writes — no reply to the client, no re-
categorization. At most one close per ticket. Notes are plain text (PSA compatibility).
```
