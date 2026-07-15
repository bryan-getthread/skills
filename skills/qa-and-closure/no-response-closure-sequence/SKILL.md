---
name: No-Response Closure Sequence
description: Close out a ticket whose client has gone silent after three documented contact attempts — send the templated final message, close with a note inviting reopen, and refuse to close anything with fewer attempts on record.
category: QA & Closure
tools: [search_tickets, add_ticket_note, update_ticket, list_ticket_statuses]
---

# No-Response Closure Sequence

The clean, defensible end of the follow-up cadence. Verifies the attempt history actually exists in the thread, sends a courteous final message, and closes with an explicit invitation to reopen — so no client ever discovers their ticket died silently.

## When to use

- A ticket has completed the follow-up cadence (three attempts) with no client reply and needs to be closed properly.
- "Close out the no-response tickets" after a cadence sweep.
- A tech asks to close a quiet ticket — this sequence proves it is safe to.
- Embedded in a Flow after the final cadence step ages out (see Unattended variant).

## Steps

1. Retrieve the ticket with `search_tickets` and verify the precondition: at least three **documented, client-visible contact attempts** since the client last replied, reasonably spaced (per the desk cadence). Phone attempts count only if a note records them. If fewer than three are on record, stop and route back to the Stale Ticket Follow-Up Cadence skill instead.
2. Confirm nothing changed since the last attempt: no inbound client reply on any channel recorded in the thread, no future scheduled appointment, no open vendor dependency. Any of these voids the closure — a client reply after attempt three means the ticket is active again.
3. Send the templated final message as a client-facing note with `add_ticket_note`. House template shape: reference the issue in one line, list the dates we reached out, state the ticket is being closed due to no response, and say plainly that replying to this message (or contacting the desk) will reopen it — no penalty, no friction. Friendly, never passive-aggressive.
4. Close the ticket with `update_ticket` using the desk's no-response closed status where one exists (check `list_ticket_statuses`); otherwise the standard closed status.
5. Add an internal closure note (plain text, no markdown): "Closed - no response after 3 documented attempts on <date>, <date>, <date>. Client invited to reopen." The attempt dates come from the thread, never from memory.
6. Report: ticket closed, attempts cited, reopen path stated.

## Guardrails

- Three documented attempts is a hard floor, not a guideline. Never close on "we probably followed up" — if the attempts are not in the thread, they did not happen.
- Never bulk-close a list of no-response tickets without per-ticket verification of the attempt history and explicit user sign-off on the batch.
- The final message and internal note must never fabricate dates, attempt counts, or contact methods.
- If a dismissive one-word client reply exists ("fine", "ok"), this is not a no-response closure — route to Ticket QA Review, which handles resolution confirmation.
- Direct email sending (`send_client_email`) is available only to the in-app agent on some tenants; the client-facing note is the standard delivery path and syncs to the PSA. Note this degradation rather than skipping the final message.
- Keep the final message localizable; match the client's language where the thread is not in English.

## Unattended (Flows) variant

- Trigger: waiting-status age timer after the final cadence step, or a "no response - pending closure" status.
- Your entire reply is the client-facing final message, posted verbatim — no narration. The status change to closed happens via tool call, then stop.
- Deterministic rules: proceed only when three documented client-visible attempts exist in the thread AND no inbound reply followed them AND no future schedule entry exists. If any check fails or the history is ambiguous, do nothing and stop — a wrongly closed ticket costs more than a stale one.
