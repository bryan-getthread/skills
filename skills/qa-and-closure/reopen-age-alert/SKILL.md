---
name: Reopen Age Alert
description: When a ticket reopens, check how long ago it was closed at run time and, if the prior closure is older than a threshold, alert the team via Teams and an internal note — a stale reopen usually means a new problem wearing an old ticket's number. Answers the commonly requested "flag reopens of long-closed tickets" partner workflow.
category: QA & Closure
tools: [search_tickets, add_ticket_note, "zapier: Microsoft Teams \"Send Channel Message\""]
---

# Reopen Age Alert

A ticket reopening days after closure is normal follow-up; a ticket reopening months later is almost always a new issue riding an old thread — and it inherits stale context that misleads whoever picks it up. This skill fires on the reopen event, reads how old the prior closure is *at that moment*, and if it exceeds a threshold, alerts the team.

## When to use

- Very old tickets get reopened by a stray reply and land back in the queue with stale context.
- "Tell us when someone reopens a ticket we closed ages ago."
- Catching reopens that should really be fresh tickets.

## Steps

1. On the reopen, read the ticket with `search_tickets`: the prior closure date/time and the reopening message.
2. Compute the age of the prior closure **at run time** (now minus closure date) and compare it to the desk's configured threshold (e.g. "older than 30 days"). If it's within the threshold, do nothing — this is ordinary follow-up.
3. If it exceeds the threshold, post a plain-text internal note via `add_ticket_note`: closed on <date>, reopened now (<age> later), and a prompt to consider splitting into a new ticket rather than reusing stale context.
4. Send a compact alert via `zapier: Microsoft Teams "Send Channel Message"` to the desk's QA/dispatch channel: ticket reference, client, closure age, one-line reopening reason.

## Guardrails

- **Age is read at run time, not used as a trigger.** The trigger is the reopen *event*; the threshold is evaluated inside the run. This skill does not — and cannot — fire "N days after" anything.
- Alert, don't act: this skill notifies. It does not re-close, split, or re-route the ticket — a human decides whether it should be a new ticket.
- Dedupe: if a reopen-age alert note already exists for this reopen, do not ping again.
- **Member-connected connector:** the Teams Zapier action runs as the member who connected it; prefer a service member for Flow use, and open with the verify-first gate (`skills/connectors/zapier-action-discovery`).
- **Task-cost + degradation:** each Teams message spends Zapier budget; if the action is unavailable, still post the internal note and say the ping was skipped.
- Notes plain text (PSA compatibility).

## Cross-references

- Pattern analysis across the reopened population: `skills/qa-and-closure/reopen-forensics`
- Distinguishing courtesy-reply reopens: `skills/triage-and-routing/re-fw-reopen-detection`

## Unattended (Flows) variant

- Fires on the **reopen event** (status change from a closed-family status back to a working status) — a supported ticket event. Not a timer.
- Runs as a Super Magic agent (a Flow cannot natively send Teams; it calls Run Skill / New Super Magic Agent).
- Your entire reply is the internal note, verbatim: `STALE REOPEN. Closed <date>, reopened <age> later. Consider a new ticket. Teams: <channel | skipped>.` or `NO ACTION (reopen within threshold).`
- Deterministic stops: closure age within threshold → no action; already alerted for this reopen → no action; connector unavailable → note only.
