---
name: Zapier Teams Approval Gate
description: Use Teams "Send Approval Request and Wait" as the human-in-the-loop gate before privileged or irreversible actions — offboarding, admin changes, out-of-contract spend. Use when a workflow needs a real named-human sign-off in Teams before proceeding.
category: Connectors
tools: [search_tickets, add_ticket_note, update_ticket, "zapier: Microsoft Teams"]
---

# Zapier Teams Approval Gate

A hard stop with a paper trail: `zapier: Microsoft Teams "Send Approval Request and Wait"` puts an approve/reject card in front of a named approver and blocks until they answer. The privileged action happens only on an explicit approve — and the outcome lands on the ticket either way.

## When to use

- Offboarding steps that disable a person's account or forward their mailbox.
- Any client-billable or out-of-contract work before it's performed (pairs with Zapier QuickBooks Time & Billing).
- Config changes a client contact must sign off on and the desk wants answered in Teams rather than email.

## Steps

1. Name the gated action precisely before requesting anything: what will happen, to whom/what, and its reversibility. A vague approval approves nothing.
2. Resolve the approver: the client's authorized contact or internal manager per the tenant's policy — from the ticket/contact record, never from who happens to be online. Wrong approver = no gate at all.
3. Compose the approval request: the exact action, the subject (<user>, <system>), the source ticket reference, and any cost. It must be decidable from the card alone.
4. Show the request + approver for confirmation, then send via `zapier: Microsoft Teams "Send Approval Request and Wait"`.
5. Branch strictly on the response:
   - **Approved** → proceed with the gated action; `add_ticket_note` (plain text): what was approved, by whom, when, and via Teams approval.
   - **Rejected** → do NOT proceed; note the rejection and route the ticket back to the requester/member.
   - **Timeout / no response** → treat exactly as not-approved. Never proceed on silence; note the non-response and follow up or re-request.
6. Only after the approved action completes, record completion separately — approval received and action done are two different facts and two different notes.

## Guardrails

- **Member-connected Zapier required:** without it, degrade to Thread's native `send_approval`, or stop and tell the member no approval channel is available — never skip the gate because the tool is missing.
- Silence is a no. Timeouts, ambiguous replies, and "approved" from anyone other than the designated approver all mean the action does not run.
- The approval record on the ticket is mandatory and plain text — it is audit evidence.
- One approval covers one action. Batching ten offboardings under one approve card destroys the gate's meaning; per-action requests unless the member explicitly defines a batch.
- Task cost: each Zapier call = 2 tasks; the wait itself doesn't multiply cost, but don't re-send on every check-in — re-request only after a genuine timeout.
- Unattended use inside flows must follow Unattended Output Discipline: on any uncertainty about approver identity or scope, stop and do nothing.
