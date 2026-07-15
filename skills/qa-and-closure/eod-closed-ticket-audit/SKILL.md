---
name: EOD Closed Ticket Audit
description: End-of-day quality sweep of every ticket closed today — check each against the closure rubric (resolution evidence, documentation, classification, time, closure message) and produce a pass/fail summary with reopen recommendations.
category: QA & Closure
tools: [search_tickets, add_ticket_note, update_ticket, list_boards]
---

# EOD Closed Ticket Audit

The daily batch version of the QA gate: instead of grading one ticket at close time, it sweeps everything closed today and reports where the closure standard slipped — before the tickets are a week old and nobody remembers the context.

## When to use

- End of day: "audit today's closed tickets" / "how did today's closures look?"
- A lead running a daily quality pulse on the desk or on one tech's closures.
- Catching up after a busy day when the per-ticket QA gate was not running.

## Steps

1. Find every ticket closed today with `search_tickets`, splitting searches per board (`list_boards`) so result caps do not truncate the day. If a search may have capped, say so in the output rather than presenting the count as exact.
2. For each closed ticket, grade the closure rubric strictly from the thread (same criteria as Ticket QA Review): genuine resolution evidence (customer confirmation or documented verbal confirmation — a work summary does not count), classification set, owner assigned, time logged, title accuracy, and a client-facing closure message. Additionally check **documentation**: the notes let a stranger reconstruct what the issue was, what was done, and why it worked.
3. Score each ticket PASS (all criteria met) or FAIL (list the failing criteria). Score 0 on any criterion whose evidence is not in the thread — never assume.
4. Produce the audit report:
   - Headline: N closed, N passed, N failed, pass rate.
   - Failures grouped by ticket: failing criteria, one line each, plus the single fix that would make it pass.
   - Patterns worth a sentence: a criterion failing repeatedly, or a cluster on one board.
5. For each failure, recommend the action: reopen for rework, or annotate-and-leave when the miss is documentation-only and the client outcome was fine. Apply reopens (`update_ticket` back to a working status + plain-text internal note via `add_ticket_note`, in that order) only for tickets the user approves.
6. Keep the whole report skimmable in under a minute — a table plus a short patterns paragraph, not an essay.

## Guardrails

- Present the audit before touching any ticket; never reopen in bulk without explicit sign-off on the specific tickets.
- Grade from the thread only. If confirmation may have happened off-channel, mark it FAIL on evidence and flag for the tech to document — do not give benefit of the doubt.
- Per-tech numbers in the report are factual counts, not commentary; coaching interpretation belongs to the Weekly QA Digest skill and the human lead.
- Notes posted to reopened tickets are plain text (no markdown or emojis) for PSA sync compatibility.
- Disclose result caps; a partial audit presented as complete is worse than no audit.
- If write tools are disabled for this tenant, deliver findings and recommended reopens in chat only.
