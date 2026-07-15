---
name: Queue Hygiene Score
description: Scan a queue or board for hygiene defects — missing contacts, stale or wrong statuses, tickets with no notes, unassigned owners, blank classifications — and return a hygiene score with a concrete fix list.
category: QA & Closure
tools: [search_tickets, list_boards, list_ticket_statuses, search_contacts, update_ticket, add_ticket_note]
---

# Queue Hygiene Score

A queue's data quality decays quietly: contacts missing, statuses lying, notes empty. This skill measures the decay as a single hygiene score and hands back the ranked fix list — so "clean up the board" becomes a checklist instead of a vibe.

## When to use

- "How clean is the <board> queue?" / "give me a hygiene score for the service board"
- Before dispatch automation or SLA reporting goes live (both are garbage-in-garbage-out on dirty queues).
- A recurring weekly hygiene check comparing score to last week.
- After an alert storm or migration that likely left malformed tickets behind.

## Steps

1. Scope the scan: which board(s) (`list_boards`) and which statuses count as open. Enumerate open tickets with `search_tickets`, splitting searches **per defect signal per board** so result caps do not hide defects; disclose any capped search in the output.
2. Check each hygiene signal as its own pass:
   - **Missing contact** — no contact, or a catchall/placeholder contact.
   - **Missing or implausible company** — unassigned company on a board that requires one.
   - **Stale status** — status contradicts the thread (e.g. "scheduled" with no schedule entry; "waiting on client" where the last message is inbound; "new" but worked for days). Use `list_ticket_statuses` to map the board's status semantics.
   - **No notes** — open beyond a grace window (default 2 business days) with zero notes beyond the intake message.
   - **No owner** — unassigned beyond the same grace window.
   - **Blank classification** — type/subtype or priority unset.
   - **Ancient never-touched** — tickets so old and untouched they are probably noise candidates.
3. Compute the hygiene score: percentage of open tickets with zero defects, plus a per-signal defect count. Simple and repeatable beats clever — the score's value is week-over-week comparison, so state the formula in the output.
4. Build the fix list, ranked by effort-to-impact: quick wins first (set owner, set classification), then judgment fixes (correct stale statuses — link each to what the thread implies), then candidates for other skills (stale follow-ups → Stale Ticket Follow-Up Cadence; missing companies → catchall routing; noise → closure with sign-off).
5. Apply fixes only on explicit approval, ticket by ticket or as an approved batch: `update_ticket` for owner/status/classification, `search_contacts` + contact assignment for resolvable missing contacts, `add_ticket_note` (plain text) where a correction needs context. Never fix silently.
6. Output: score headline, per-signal table, top fix list, and — when a previous score is provided — the trend.

## Guardrails

- Diagnose freely, fix only with sign-off; a bulk status "correction" applied wrong is a worse mess than the dirt it replaced.
- Contact/company fixes follow the catchall rule: assign only on an unambiguous match; name similarity is never enough — low confidence means flag, not fix.
- Result-cap honesty is structural here: per-signal-per-board splits are mandatory, and any capped count is labeled "at least N".
- The score measures data hygiene, not tech performance — do not present it as a people metric.
- If write tools are disabled for this tenant, deliver score and fix list in chat only.
