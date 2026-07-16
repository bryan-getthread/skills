---
name: Automation Completion Auto-Close
description: When note evidence shows an intent or external automation actually finished the work, verify it and close the ticket with an audit note — aborting the moment any human message arrives after completion. Answers the commonly requested "auto-close tickets the automation already handled" partner workflow.
category: Automation & Flows
tools: [search_tickets, update_ticket, add_ticket_note, list_ticket_statuses]
---

# Automation Completion Auto-Close

Intents and external automations often do the whole job and leave the ticket sitting open. This skill closes those tickets — but only after confirming from note evidence that the automation genuinely completed, and only if no human has said anything since. When in doubt, it leaves the ticket open.

## When to use

- An intent or automation resolves a request end-to-end (password reset, provisioning step) and the ticket lingers open.
- "Close the ones the automation already finished."
- A flow should sweep automation-handled tickets to a closed status once the completion note lands.

## Steps

1. Re-fetch the ticket with `search_tickets` and read the full thread, newest-first.
2. Find explicit completion evidence in the notes: a success message from the intent/automation, a "completed / done / provisioned" system note tied to this ticket's request — not a "started" or "queued" note, and not a client asking for the thing. If there is no unambiguous completion note, stop.
3. **Abort on any human message after completion.** If a technician or client posted anything (question, correction, new request) after the completion note, the ticket is live again — do nothing.
4. Confirm the target closed/resolved status exists via `list_ticket_statuses` (respect the desk's closed-status taxonomy — see `skills/psa-specific/psa-closed-status-taxonomy`).
5. Close with `update_ticket` to the resolved/closed status and post a plain-text audit note via `add_ticket_note`: which automation completed, the note it was evidenced by, and that no human message followed.

## Guardrails

- **Evidence gate:** close only on a genuine completion note. A "started"/"in progress" note, a client request, or an ambiguous system message is not completion. When unsure, leave it open — a false close is worse than a lingering open ticket.
- **Human-after-completion abort is absolute.** Any human message posted after the completion evidence cancels the close.
- Never fabricate completion. Do not infer "it probably worked" from silence.
- Status change and the audit note are the only writes. No reply to the client, no re-categorization here.
- Notes are plain text (PSA compatibility).

## Unattended (Flows) variant

- Fires on the **note-added event** (the completion note landing) — a supported ticket event, not a timer or "N minutes after".
- Your entire reply is the audit note, verbatim: `AUTO-CLOSED. Automation: <name>. Evidence: <note ref>. No human message after completion.` or `NO ACTION. Reason: <no completion note | human message after completion | status missing>.`
- Deterministic stops, in order: no completion note → no action; human message after completion → no action; target status missing on tenant → no action.
- At most one close per ticket. Never close on inferred or partial completion.
