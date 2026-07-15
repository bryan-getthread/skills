---
name: Courtesy Reply Status Revert
description: When a thanks-only client reply flips a resolved ticket back to a responded/open status, revert it to the correct status per a fixed per-board map — the revert is the only permitted write.
category: Triage & Routing
tools: [search_tickets, update_ticket, list_ticket_statuses]
---

# Courtesy Reply Status Revert

"Thanks so much!" should not resurrect a resolved ticket. When a courtesy-only reply moved the status, this skill puts the status back according to a fixed per-board mapping — and does nothing else.

## When to use

- A flow fires on customer-responded status changes to catch courtesy flips.
- Resolved tickets keep reappearing in the active queue because clients say thanks.
- "Revert the status if that last reply was just a thank-you."

## Steps

1. Read the ticket with `search_tickets`: current status, prior status, board, and the message that triggered the status change.
2. Confirm the triggering message is courtesy-only: gratitude, acknowledgment, or sign-off with zero questions, zero requests, zero new symptoms, and no attachment presented for action. Evaluate the entire message, in its language (courtesy phrases vary by locale).
3. Look up this board in the fixed per-board revert map configured for this skill (e.g. "help desk board: customer-responded reverts to resolved-pending-close"). Verify the target status exists via `list_ticket_statuses`.
4. If the message is courtesy-only AND the board has a mapping AND the ticket was flipped from a resolved-family status: revert with `update_ticket` to the mapped status.
5. Otherwise, do nothing.

## Guardrails

- WHEN IN DOUBT, DO NOTHING. Any hint of a question, complaint, new symptom, or "but…" means the reply is real — leave the status alone.
- The status revert is the ONLY permitted write. No notes, no replies, no priority/owner/board changes, no closing.
- Only revert flips out of resolved-family statuses; never touch tickets that were genuinely in progress.
- Boards not present in the map are out of scope — no improvised mappings.
- Revert at most once per triggering message; if the client replies again after the revert, do not revert again without a new courtesy-only evaluation.

## Unattended (Flows) variant

- Reply with exactly one line (it is logged, not posted to the ticket): `REVERTED #<n> to <status> per board map.` or `NO ACTION.` — nothing else, no narration.
- Deterministic stops: message not courtesy-only → NO ACTION; board unmapped → NO ACTION; prior status not resolved-family → NO ACTION; mapped status missing on tenant → NO ACTION.
- Never add a note or send anything to the client in this variant.
