---
name: Timepad Outreach Status Update
description: Read a ticket's latest time-entry note and, if it signals off-thread customer outreach ("called client, left VM"), clear the ticket's awaiting-response state — behind a confidence gate, on confirmation. Answers the commonly requested "my call didn't update the ticket status" workflow; runs manually on demand (Flows can't trigger on a time-entry submission).
category: QA & Closure
tools: [search_tickets, update_ticket, list_ticket_statuses]
---

# Timepad Outreach Status Update

Techs log a call in the time entry ("phoned the client, left a voicemail") but the ticket still sits in an awaiting-response / needs-attention state, because the outreach happened off-thread. Read the latest time-entry note, detect that outreach happened, and move the status to match — carefully.

## When to use

- "I logged a call on this — update the status to reflect it"
- "The time entry says I reached out; clear the awaiting-response flag"
- "Did my last time entry mention outreach? If so, fix the status"
- Reconciling ticket status with off-thread contact captured only in a time note

## Steps

1. Load the ticket and read its **latest** time-entry note (search_tickets / the ticket thread).
2. Classify the note for an outreach signal: does it clearly describe the tech contacting the customer off-thread — "called", "left VM", "spoke with", "emailed directly", "reached out"? Distinguish that from internal work ("investigated", "rebooted server") which is NOT customer outreach.
3. **Confidence gate.** Only proceed if the outreach signal is clear. If the note is ambiguous, internal-only, or you're inferring, do nothing but report why — never flip a status on a guess.
4. Determine the correct target status (e.g. from "Awaiting Response" / new to "Waiting on Client") and confirm that status exists via list_ticket_statuses.
5. Preview the change: "Latest time entry says '<quoted signal>' → propose moving #<n> from <current> to <target>." Wait for an explicit yes.
6. On confirm, update_ticket to the new status and report it. If the member declines or the gate failed, leave the ticket untouched.

## Guardrails

- Confidence gate is the whole point: when in doubt, do nothing and say why. A wrong status flip hides a ticket that still needs a human.
- Confirm before writing — never change status without an explicit go-ahead on the previewed, quoted evidence.
- Quote the exact time-entry text that justified the change so the decision is auditable; never fabricate an outreach signal.
- Only act on genuine *customer* outreach — internal work notes must never trigger a status change.
- Validate the target status against list_ticket_statuses before setting it.
- **Manual only.** Thread Flows fire on ticket events and conditions — there is no time-entry-submission trigger and no elapsed-time trigger — so this cannot be a flow. Run it on demand.
