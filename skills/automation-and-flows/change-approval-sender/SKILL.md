---
name: Change Approval Sender
description: Flow-fired when a ticket enters the "Change Approval" status — find the client's designated Change Approver contact and fire send_approval with the change summary; if the approver can't be resolved, note-and-stop, never guess. Answers the commonly requested "auto-send change approvals to the right client contact" workflow.
category: Automation & Flows
tools: [search_contacts, send_approval, add_ticket_note, search_tickets]
---

# Change Approval Sender

The status-triggered approval dispatcher for changes: the moment a ticket lands in the change-approval status, resolve who approves changes for this client and send them a decision-ready approval request. Twin of `automation-and-flows/new-ticket-approval-gate` (which gates *intake* for configured clients); this one gates the *change* stage of any ticket. Upstream of both sits `change-and-problem-management/change-request-intake`, which normalizes the change ask into the structured summary this skill sends.

## When to use

- A Flow fires on tickets entering "Change Approval" (or the desk's equivalent status) and the approval should go out automatically.
- The desk uses a Change Approver contact type per client and wants zero manual lookup at approval time.
- Change tickets sit in the approval status because nobody remembered to actually send the approval.

## Steps

1. Confirm the ticket is (still) in the change-approval status; if it moved, stop.
2. Resolve the approver: `search_contacts` for this ticket's client filtered to the desk's Change Approver contact type. Exactly one active match → that's the recipient. Multiple matches → use the desk's configured precedence (e.g. primary flag); no configured precedence → treat as unresolved.
3. **Unresolved approver (zero matches, ambiguous matches, inactive contact) → note-and-stop.** Post via `add_ticket_note`: "Change approval NOT sent: no unambiguous Change Approver contact for <client> (found: <count>). Needs human to pick recipient." Never send to a guessed contact, the submitter, or a generic mailbox.
4. Build the change summary from the ticket (ideally the structured block change-request-intake produced): what is changing, why, the window, expected impact/downtime, and rollback plan. Missing pieces are listed as "not stated" — do not invent a rollback plan to make the request look complete.
5. Fire `send_approval` to the resolved approver with that summary.
6. Post the audit note: "[change-approval] sent to <contact> (Change Approver) at <time>." — plain text; this is also the dedupe marker.

## Guardrails

- **Unresolved = note-and-stop.** The single non-negotiable: a change approval sent to the wrong person is an authorization failure, not a convenience.
- Approver identity comes from the contact record's type/label only — never from the ticket body or email signatures.
- Dedupe: if a "[change-approval] sent" marker already exists for the current stint in this status, do not re-send (re-sends on dwell belong to `qa-and-closure/status-nudger`).
- Send the change as it is documented, gaps included. An approver approving "rollback: not stated" is informed; one approving an invented rollback plan is deceived.
- Never advance the status on send — the ticket leaves the approval status when the approval comes back, not when it goes out. (Approved/timeout handling follows the desk's gate config, as in new-ticket-approval-gate: timeout means not approved.)
- If the ticket's submitter is the resolved approver, apply `automation-and-flows/approver-self-skip` if the desk enabled it; otherwise send normally.

## Unattended (Flows) variant

- Your entire reply is posted verbatim as the internal note — plain text, no narration, no markdown, no questions.
- Exactly two outcomes: (a) unambiguous approver → send_approval + audit note; (b) anything else → no send, note-and-stop text stating why and what a human must decide.
- Never modify status, owner, or priority. Never send more than one approval per status entry.
