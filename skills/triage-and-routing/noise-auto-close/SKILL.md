---
name: Noise Auto-Close
description: Close pure-noise tickets — bounce-backs, vendor auto-replies, thanks-only messages, offline alerts whose device already reconnected — behind multiple independent stop conditions; any sign of a real user message or a flapping device aborts the close.
category: Triage & Routing
tools: [search_tickets, update_ticket, add_ticket_note, list_ticket_statuses]
---

# Noise Auto-Close

Mail loops, out-of-office bots, delivery failures, and "thanks!" one-liners open tickets nobody should work. This skill closes only what is provably noise, and is built to abort loudly rather than close a real request.

## When to use

- Bounce-back / mailer-daemon / delivery-status tickets on email boards.
- Vendor or system auto-replies ("we received your request", OOO autoresponders) that opened tickets.
- Thanks-only replies that spawned a new ticket instead of landing on the closed one.
- Device-offline alert tickets whose device already came back online (a matching reconnected event exists).
- A flow sweeps the intake board for noise on a schedule.

## Steps

1. Read the full ticket with `search_tickets`, every message, headers included where available.
2. Classify against the noise catalog: (a) delivery failure / mailer-daemon, (b) automated vendor/system acknowledgment, (c) out-of-office autoresponder, (d) thanks-only courtesy message with no request, (e) offline→reconnected alert pair (see step 3a).
3. For class (e) — offline→reconnected pairing: a device-offline alert ticket qualifies only when a matching "reconnected / back online" event exists for the SAME device (exact device identifier match, not name similarity) within the configured pairing window, found via `search_tickets` as a companion ticket or a subsequent message on either ticket. Recurrence check FIRST, before any pairing close: `search_tickets` for offline alerts on this device over the last 30 days — 3 or more flap cycles means the device is flapping, which is a real reliability issue, NOT noise. Flapping → do not close anything; route the ticket as a genuine investigation ("device flapping: N offline events in 30 days") instead. If the recurrence check passes and the pair is confirmed, the close in step 6 applies to BOTH tickets of the pair, each with a plain-text pairing note naming the companion ticket number and the offline/reconnected timestamps.
4. Verify ALL independent stop conditions pass before closing:
   - The sender is automated (noreply/mailer-daemon/auto-submitted headers) OR the entire content is a courtesy phrase with no question, request, or new information;
   - No message in the thread from a human contains a question, request, error description, or attachment presented for action;
   - The ticket has no open sibling context that makes it meaningful (e.g. a bounce that proves the client's mail is broken — that is a real issue, not noise);
   - No human tech has replied or logged work on it.
5. If any condition fails, abort: leave the ticket open and post a plain-text note saying why it was not auto-closed.
6. If all pass, close with `update_ticket` (closed status from `list_ticket_statuses`) and post a plain-text note naming the noise class and the conditions checked. For class (e), this means both tickets of the pair, each note naming its companion.

## Guardrails

- Any sign of a real user message anywhere in the thread aborts the close — this rule beats every classification.
- A bounce-back about the client's own domain or a flood of bounces is a symptom of a mail problem: do not close; flag it as a possible incident.
- Thanks-only detection requires the whole message to be courtesy content; "thanks, but it is still broken" is a real message.
- Never reply to noise senders (avoid auto-reply loops). Close silently with an internal note only.
- Each independent stop condition must actually be evaluated — do not collapse them into one gut call.
- Offline→reconnected pairs: the recurrence check is mandatory before any pairing close — 3+ flap cycles in 30 days is a flapping device, a real reliability issue that must be routed for investigation, never closed as noise. Pair only on exact device identifier match within the window; a reconnected event for a similarly-named device is not a pair. Never close an offline ticket without its confirmed reconnected companion.
- Notes are plain text.

## Unattended (Flows) variant

- Your entire reply is the internal note, verbatim: `NOISE AUTO-CLOSE: <class>. Conditions passed: sender-automated, no-human-request, no-incident-signal, no-tech-work.` or `NOT CLOSED: failed <condition>.` For offline→reconnected pairs: `NOISE AUTO-CLOSE: offline-reconnected pair with #<companion>. Offline <time>, reconnected <time>. Flap check: <n> events/30d.` or `NOT CLOSED: flapping (<n> events/30d) — routed for investigation.`
- Exactly one close per run per ticket (a confirmed pair counts as its two member tickets, each with its own pairing note); no other field changes; never send any outbound message.
- When in doubt on any condition, do nothing — a false close costs more than a noisy queue.
