---
name: Closure Intent Auto-Resolve
description: When a customer reply clearly says they're satisfied or asks to close the ticket, resolve it with a closing note — under a high-confidence gate, doing nothing whenever there's any doubt. Answers the commonly requested "close tickets the client already told us to close" partner workflow.
category: QA & Closure
tools: [search_tickets, update_ticket, add_ticket_note, list_ticket_statuses]
---

# Closure Intent Auto-Resolve

When a client says "that's sorted, thanks — you can close this," the ticket often sits open anyway. This skill resolves tickets on an explicit closure request or unambiguous satisfaction — behind a strict confidence gate. Anything short of a clear close signal, and it leaves the ticket alone.

## When to use

- Clients explicitly confirm the issue is resolved or ask to close, but tickets linger open.
- "Close it when the customer says it's fixed."
- Trimming the queue of tickets the client has already signed off on.

## Steps

1. Read the reply and the thread with `search_tickets`.
2. Apply the **high-confidence gate**: resolve only on an explicit closure/satisfaction signal — "you can close this", "all set now", "issue is resolved, thanks" — with no open question, no partial ("mostly, but…"), no new symptom. A bare "thanks" is *not* a close signal (see `skills/triage-and-routing/courtesy-reply-status-revert` and `skills/qa-and-closure/premature-confirmation-detector`).
3. Confirm the target resolved/closed status exists via `list_ticket_statuses` and respects the desk's closed-status taxonomy (`skills/psa-specific/psa-closed-status-taxonomy`).
4. If the gate passes, resolve with `update_ticket` and post a plain-text closing note via `add_ticket_note`: resolved per the client's explicit confirmation, quoting the closure phrase.
5. Otherwise, do nothing.

## Guardrails

- **When in doubt, do nothing.** This is the core rule. Ambiguity, partial satisfaction, a lingering question, or a mere "thanks" all mean leave it open. A false close annoys clients and inflates reopens.
- Never fabricate a confirmation — the closure signal must be in the client's own words on the thread.
- Do not close if the last message contains any new request or unresolved symptom, even alongside thanks.
- Status change + one closing note are the only writes. No client-facing reply is sent by this skill (an optional acknowledgement can be drafted separately — see `skills/communication/resolution-closing-email`).
- Notes plain text (PSA compatibility).

## Unattended (Flows) variant

- Fires on the **reply event** — a supported ticket condition.
- Your entire reply is the closing note, verbatim: `RESOLVED per client confirmation: "<quoted phrase>".` or `NO ACTION.`
- Deterministic stops: no explicit close/satisfaction signal → no action; any open question or new symptom → no action; bare thanks → no action; target status missing → no action.
- Resolve at most once per qualifying reply. When uncertain, do nothing.
