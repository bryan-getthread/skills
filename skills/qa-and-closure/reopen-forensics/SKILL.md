---
name: Reopen Forensics
description: Analyze why tickets reopen — patterns by technician, client, and issue type over a window, with representative examples and the distinction between premature closure, recurrence, and client-initiated reopens.
category: QA & Closure
tools: [search_tickets, search_members, search_clients]
---

# Reopen Forensics

Reopens are the desk's most honest quality signal: every one means a closure that did not hold. This skill reads the reopened population for a window and explains the why — which failure mode, whose tickets, which clients, which issue types — with real examples attached.

## When to use

- "Why are our tickets reopening?" / "analyze reopens for the last 90 days"
- A lead investigating a rising reopen rate or a specific tech's reopen cluster.
- Feeding root-cause input into the QA rubric or training plan.
- "Which clients keep coming back about the same thing?"

## Steps

1. Find reopened tickets in the window with `search_tickets`: tickets that left a closed status and returned to a working status, or closed tickets whose thread shows a post-closure inbound reply / RE: on the same reference. Split searches per board and disclose result caps.
2. For each reopen, read the thread around the closure and classify the failure mode from evidence:
   - **Premature closure** — closed without customer confirmation and the client came back saying it was never fixed.
   - **Recurrence** — genuinely fixed, then the same symptom returned (fix did not hold, or root cause untreated).
   - **Incomplete scope** — the original request had parts that were never done.
   - **Client-initiated non-failure** — "thanks" replies reopening the ticket, a new unrelated issue on an old thread. These are hygiene noise, not quality failures; count them separately.
3. Aggregate by three axes: technician (`search_members`), client (`search_clients`), and issue type/subtype. For each axis, surface the concentrations — a tech whose reopens are mostly premature closures, a client with repeated recurrences of one issue, an issue type that reopens desk-wide.
4. Attach 2–3 representative examples per major pattern: ticket number, one-line story ("closed after work summary, no confirmation; client replied 2 days later — printer still offline"). Examples come only from tickets actually read; never invent them.
5. Output the forensics report: headline reopen rate (with noise reopens excluded and shown separately), failure-mode breakdown, the top patterns per axis with examples, and 2–3 recommendations mapped to the findings (e.g. "premature closures dominate → enforce the QA gate's confirmation criterion on <board>"; "recurrences on <issue type> → route to root-cause analysis / automation candidate").

## Guardrails

- Classify only from thread evidence; when the failure mode is ambiguous, label it AMBIGUOUS rather than guessing — do not silently inflate any category.
- Separate hygiene noise ("thanks" reopens) from quality failures before computing any rate; a courtesy-reply reopen is not a tech's failure.
- Per-tech findings are patterns for coaching, not verdicts; wording stays neutral and the report notes sample sizes (3 reopens is an anecdote, not a pattern).
- Read-only: this skill does not reopen, close, or annotate tickets.
- Disclose result caps and the window actually covered; a reopen rate from a truncated sample must say so.
