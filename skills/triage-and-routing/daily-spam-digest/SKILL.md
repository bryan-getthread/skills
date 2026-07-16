---
name: Daily Spam Digest
description: Scheduled digest of the spam and noise tickets auto-closed in the last window, grouped by matched pattern, with borderline closes flagged for human false-positive review — the audit trail for what the unattended closers did.
category: Triage & Routing
tools: [search_tickets, list_boards, list_ticket_statuses]
---

# Daily Spam Digest

Unattended closers (spam-sender-triage, noise-auto-close) work in silence — this skill is their daily accountability report. It sweeps the window's auto-closed tickets, groups them by the pattern that closed them, and puts the borderline ones in front of a human with a direct question: were these actually noise?

## When to use

- A scheduled flow runs each morning to report what the spam/noise closers did overnight.
- "What got auto-closed yesterday and why?"
- A tech suspects a real ticket was eaten by the spam filter and wants the recent close list.
- Periodic audit of auto-close pattern quality (which patterns fire most, which misfire).

## Steps

1. Establish the window: since the last digest run (default: previous 24 hours). State the window explicitly in the output.
2. Find the auto-closed population with `search_tickets`: tickets moved to a closed-family status (confirm status ids via `list_ticket_statuses`) within the window whose closure note carries an auto-close marker (e.g. "NOISE AUTO-CLOSE:", "SPAM:", or the tenant's configured marker). Search per board via `list_boards` — split searches per signal per board; if any search hits a result cap, say so in the digest rather than presenting the count as exact.
3. Group by matched pattern: the noise class or spam pattern named in each closure note (delivery failure, vendor auto-reply, out-of-office, thanks-only, known-spam sender, offline/reconnected pair, etc.). Unparseable or missing markers go in an "unattributed closes" group — those are review items by definition.
4. Score each close for borderline signals: a human message anywhere in the thread, a subject that names a real system or error, a sender that is a known contact (not automated), a ticket that was open more than a few minutes before closing, or a pattern that matched on weak evidence. Any signal → flag as borderline.
5. Build the digest:
   - Header: window, total auto-closed, count per pattern group.
   - Per group: ticket numbers with one-line subjects (real numbers only — never invent ticket numbers).
   - Borderline section: "These N were borderline — confirm they were noise:" with the specific signal per ticket and the instruction to reopen any false positive manually (or via the appropriate skill).
   - Health note: any pattern group whose borderline rate looks high is called out as a tuning candidate for the closer skill.
6. Output the digest as plain text, skimmable in under a minute. This skill reports; it does not reopen, re-close, or edit tickets.

## Guardrails

- Read-only: no ticket writes of any kind. Reopening a false positive is a human decision made from the digest, not an action of the digest.
- Never invent ticket numbers or counts; if a search capped, report "at least N (result cap hit)".
- Do not re-litigate every close — only flag borderlines with a named signal. A digest that cries wolf on everything gets ignored.
- Tickets closed by humans in the window are out of scope — this audits the unattended closers only; exclude closes without an auto-close marker from the pattern groups (they may appear in "unattributed" only if the note suggests automation).
- Sister skills: spam-sender-triage and noise-auto-close do the closing; this skill audits them. Tuning suggestions point at those skills' patterns, not at ad-hoc rule changes.

## Unattended (Flows) variant

- Your entire reply is the digest, verbatim, plain text — no narration, no preamble, no markdown tables destined for a PSA.
- If zero tickets were auto-closed in the window, reply exactly: `SPAM DIGEST <window>: no auto-closes.`
- If searches fail or the closed-status lookup fails, reply with a one-line error naming the failed step — never emit a partial digest that looks complete.
