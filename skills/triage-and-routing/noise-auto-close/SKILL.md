---
name: Noise Auto-Close
description: Close pure-noise tickets — bounce-backs, vendor auto-replies, thanks-only messages — behind multiple independent stop conditions; any sign of a real user message aborts the close.
category: Triage & Routing
tools: [search_tickets, update_ticket, add_ticket_note, list_ticket_statuses]
---

# Noise Auto-Close

Mail loops, out-of-office bots, delivery failures, and "thanks!" one-liners open tickets nobody should work. This skill closes only what is provably noise, and is built to abort loudly rather than close a real request.

## When to use

- Bounce-back / mailer-daemon / delivery-status tickets on email boards.
- Vendor or system auto-replies ("we received your request", OOO autoresponders) that opened tickets.
- Thanks-only replies that spawned a new ticket instead of landing on the closed one.
- A flow sweeps the intake board for noise on a schedule.

## Steps

1. Read the full ticket with `search_tickets`, every message, headers included where available.
2. Classify against the noise catalog: (a) delivery failure / mailer-daemon, (b) automated vendor/system acknowledgment, (c) out-of-office autoresponder, (d) thanks-only courtesy message with no request.
3. Verify ALL independent stop conditions pass before closing:
   - The sender is automated (noreply/mailer-daemon/auto-submitted headers) OR the entire content is a courtesy phrase with no question, request, or new information;
   - No message in the thread from a human contains a question, request, error description, or attachment presented for action;
   - The ticket has no open sibling context that makes it meaningful (e.g. a bounce that proves the client's mail is broken — that is a real issue, not noise);
   - No human tech has replied or logged work on it.
4. If any condition fails, abort: leave the ticket open and post a plain-text note saying why it was not auto-closed.
5. If all pass, close with `update_ticket` (closed status from `list_ticket_statuses`) and post a plain-text note naming the noise class and the conditions checked.

## Guardrails

- Any sign of a real user message anywhere in the thread aborts the close — this rule beats every classification.
- A bounce-back about the client's own domain or a flood of bounces is a symptom of a mail problem: do not close; flag it as a possible incident.
- Thanks-only detection requires the whole message to be courtesy content; "thanks, but it is still broken" is a real message.
- Never reply to noise senders (avoid auto-reply loops). Close silently with an internal note only.
- Each independent stop condition must actually be evaluated — do not collapse them into one gut call.
- Notes are plain text.

## Unattended (Flows) variant

- Your entire reply is the internal note, verbatim: `NOISE AUTO-CLOSE: <class>. Conditions passed: sender-automated, no-human-request, no-incident-signal, no-tech-work.` or `NOT CLOSED: failed <condition>.`
- Exactly one close per run per ticket; no other field changes; never send any outbound message.
- When in doubt on any condition, do nothing — a false close costs more than a noisy queue.
