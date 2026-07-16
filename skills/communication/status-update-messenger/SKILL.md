---
name: Status Update Messenger
description: One command sets the ticket status and posts the matching templated client message from a per-status map — so a status change and the client-facing note that should go with it happen together. Answers the commonly requested "change the status and tell the client" workflow; runs manually on demand.
category: Communication
tools: [update_ticket, list_ticket_statuses, add_ticket_note]
---

# Status Update Messenger

Moving a ticket to "Waiting on Client" or "Scheduled" or "Resolved" usually should trigger a specific message to the client — and the two often get out of sync (status flips, client hears nothing; or a message goes out but the status stays wrong). Do both from one command, using a per-status message map.

## When to use

- "Set this to Waiting on Client and tell them why"
- "Move to Scheduled and send the appointment note"
- "Mark resolved and post the closing message"
- Any status change that has a standard client-facing message attached

## Steps

1. Confirm the target status against the desk's real statuses with list_ticket_statuses (statuses vary per board/PSA — never assume a status name exists).
2. Look up the matching message in the per-status template map (the desk's standard wording for that status). Fill it with the ticket specifics (client name, next step, timeframe).
3. Preview both: the status change and the exact client-facing message text. Wait for an explicit yes. Let the member edit the message before it goes.
4. On confirm, set the status with update_ticket, then post the message — as a client reply if it's outbound, or via add_ticket_note if the desk sends manually. Keep note text plain-text for PSA compatibility.
5. Report both: "Set status to <X> and posted the <X> client message on #<n>." If either fails, say which.

## Guardrails

- Confirm before writing. A status change plus an outbound message is two consequential writes; both wait on one explicit go-ahead over the preview.
- Validate the status name against list_ticket_statuses — don't attempt to set a status that doesn't exist on that board.
- Use the mapped template for the status; if no message is mapped for the chosen status, ask rather than improvising an outbound message.
- Keep the two in sync — never set the status without the message (or vice versa) when the map calls for both; report partial success honestly if one write fails.
- No fabrication of statuses or client-facing commitments (dates, timeframes) not grounded in the ticket.
- Member-invoked only; the client-message half can't run as a native Flow action (a flow could set status, but only a Run Skill / Super Magic agent can post the paired message).
