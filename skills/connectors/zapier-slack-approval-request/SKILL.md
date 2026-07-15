---
name: Zapier Slack Approval Request
description: Use Slack "Request Approval" as the human-in-the-loop gate before privileged or irreversible actions when the desk lives in Slack rather than Teams — named approver, explicit approve/reject, silence means no. Use when a workflow needs real sign-off delivered in Slack.
category: Connectors
tools: [search_tickets, add_ticket_note, update_ticket, "zapier: Slack"]
---

# Zapier Slack Approval Request

The Slack twin of the Zapier Teams Approval Gate — same pattern, different surface: `zapier: Slack "Request Approval"` puts an approve/reject decision in front of a named approver, and the privileged action runs only on an explicit approve, with the outcome recorded on the ticket either way.

## When to use

- Offboarding, admin changes, or out-of-contract spend needing sign-off, in a Slack-first shop.
- A client-authorized contact or internal manager must approve before work proceeds and Slack is where they'll actually answer.
- Any skill that says "approval-gated" when the tenant's approval surface is Slack.

## Steps

1. Name the gated action precisely: what will happen, to whom/what, and its reversibility. A vague approval approves nothing.
2. Resolve the approver from the ticket/contact record or the tenant's policy — never from who's online in Slack. Wrong approver = no gate at all.
3. Compose the request so it is decidable from the message alone: exact action, subject (<user>, <system>), source ticket reference, any cost. Include a respond-by expectation if the action is time-boxed.
4. Show request + approver for confirmation, then send via `zapier: Slack "Request Approval"`.
5. Branch strictly on the response:
   - **Approved** → proceed; `add_ticket_note` (plain text): what was approved, by whom, when, via Slack approval.
   - **Rejected** → do NOT proceed; note the rejection and route the ticket back to the requester/member.
   - **Timeout / no response / ambiguous reply** → treat exactly as not-approved. Silence is a no. Note the non-response; re-request only after a genuine timeout.
6. Record completion separately after the approved action finishes — approval received and action done are two different facts and two different notes.

For the full pattern rationale (one approval per action, audit posture), see Zapier Teams Approval Gate — this skill is the same gate on a different transport.

## Guardrails

- **Member-connected Zapier required** (Slack). Degrade to Thread's native `send_approval`, or stop and tell the member no approval channel is available — never skip the gate because the tool is missing.
- Silence is a no. So is "approved" from anyone other than the designated approver, and so is a 👍 reaction on some other message.
- One approval covers one action; no batching ten offboardings under one approve unless the member explicitly defines the batch in the request text.
- The approval record on the ticket is mandatory, plain text, and audit-grade: action, approver, timestamp, channel.
- Task cost: each Zapier call = 2 tasks; re-request only after a real timeout, not on every check-in.
- Unattended use inside Flows follows Unattended Output Discipline: any uncertainty about approver identity or scope → stop, do nothing.
