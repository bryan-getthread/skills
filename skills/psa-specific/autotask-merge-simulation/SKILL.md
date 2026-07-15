---
name: Autotask Merge Simulation
description: For desks synced to Autotask — Autotask has no native merge API, so duplicate tickets are merged manually via a strict 5-step sequence: verify, pick survivor, cross-note both tickets, carry history, complete the source last.
category: PSA-Specific
tools: [search_tickets, add_ticket_note, update_ticket]
---

# Autotask Merge Simulation

Applies to: **Autotask (Datto/Kaseya)**. Thread's `merge_ticket` cannot merge tickets in Autotask because Autotask exposes no merge operation — a "merge" is simulated by cross-referencing and completing the duplicate. The ordering is strict: notes and history move **before** the source ticket is completed, because a completed Autotask ticket may be locked to further edits by workflow rules.

## When to use

- Two Autotask-synced tickets are confirmed duplicates and someone says "merge these".
- An alert-storm or RE:/FW: reopen produced a duplicate on an Autotask desk.
- `merge_ticket` is unavailable or fails for this tenant and the desk syncs to Autotask.

## Steps

1. **Verify the duplicate** — re-fetch both tickets at full detail with `search_tickets`. Require a strong identifier match (same alert ID, same asset, same contact + same specific error in a recent window), never wording similarity (see `skills/triage-and-routing/duplicate-hunter`). Confirm neither ticket was completed in Autotask since you last looked.
2. **Pick the survivor** — normally the older ticket, or the one with client correspondence, time entries, or SLA history. The other becomes the source. State the choice and reasoning; get confirmation before any write.
3. **Match queue and fields** — ensure the survivor sits on the correct queue with correct issue/sub-issue classification so nothing is lost when the source disappears from view. Fix with `update_ticket` if needed.
4. **Cross-note both tickets** (plain text, this syncs to Autotask):
   - On the **survivor**: "Duplicate ticket <source number> merged into this ticket on <date>. Summary of source: <requester, one-line issue, any unique detail, time worked>." Carry over any detail that exists only on the source — attachments cannot move, so name them and where they live.
   - On the **source**: "Duplicate of <survivor number>. All further work tracked there. No action will be taken on this ticket."
5. **Complete the source LAST** — only after both notes are confirmed written, set the source to the desk's duplicate-closure status via `update_ticket` (typically Complete with a "Duplicate" resolution type if the desk uses one). Never complete first: a completed source you can no longer annotate strands the history.
6. Output: survivor and source numbers, both note texts, and the source's final status. If any step fails midway, stop and report exactly which step completed — do not improvise the remainder.

## Guardrails

- **Ordering is non-negotiable:** verify → survivor → fields → cross-notes → complete source. Completing the source early is the one unrecoverable mistake.
- **Sync lag:** re-fetch both tickets immediately before step 4 and again before step 5 — a client reply landing on the source mid-merge means stop and reassess.
- Never merge on wording similarity; strong identifiers only. When in doubt, cross-reference the tickets with notes and leave both open for a human.
- Never merge a ticket that has open time entries, an active SLA target, or unanswered client messages on the source without explicit confirmation.
- All notes plain text — they sync to Autotask.
- If the client emailed both tickets, warn that replies to the source's thread may reopen it in Autotask; the desk's reopen-detection should watch for it (`skills/triage-and-routing/re-fw-reopen-detection`).

## Cross-references

- Duplicate confirmation: `skills/triage-and-routing/duplicate-hunter`, `skills/triage-and-routing/alert-storm-merge`
- Client notice: `skills/communication/duplicate-ticket-notice`
