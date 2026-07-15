---
name: CAB Brief Builder
description: Build the weekly change-board pack — pending changes ranked by risk, collision flags, and last week's change outcomes — so the CAB meeting is 20 minutes of decisions instead of an hour of reading tickets aloud.
category: Change & Problem Management
tools: [search_tickets, search_knowledge_base, add_ticket_note, list_boards, list_ticket_statuses]
---

# CAB Brief Builder

Assemble everything the change advisory board needs into one skimmable pack: what's waiting for a decision, what's riskiest, what collides, and how last week's approvals actually turned out. The outcomes section is what keeps a CAB honest — a board that never hears about failed changes learns nothing.

## When to use

- "Build the CAB pack for Thursday" / "what's on the change board this week?"
- A recurring pre-meeting prep run the day before the change review.
- A lead wants a one-off snapshot of pending change risk across clients.

## Steps

1. Establish the review window (default: changes pending decision now, plus changes executed since the last CAB — ask for the desk's cadence if unknown).
2. **Pending changes**: search_tickets on the change board for tickets in approval-pending statuses. For each, pull the one-line what/scope/window, the risk classification and scores (from change-risk-assessment — if a change reaches the pack unclassified, list it under "not ready: unassessed" rather than guessing a rank), and the prerequisites-gate status (change-request-prerequisites; incomplete requests are listed as "not ready" with the gap, not queued for decision).
3. **Risk ranking**: order the ready changes by risk — HIGH blast radius first, then LOW rollback confidence as the tiebreaker. State the ordering rule in the pack so the board knows why item 1 is item 1.
4. **Collision flags**: cross-check the proposed windows against each other and against already-scheduled changes (same systems, same clients, shared infrastructure, freeze windows — reuse the checks from change-calendar-management). Flag each collision inline on both changes involved.
5. **Last week's outcomes**: for changes executed since the last CAB, report per change: implemented as approved / implemented with deviation / rolled back / not attempted — based on ticket evidence, applying the change-completion-verification standard (qa-and-closure): a bare "done" is UNVERIFIED, not success. Include the emergency changes that bypassed the board (from emergency-change-handling) with their retro-documentation status.
6. Compose the pack, ≤1 page equivalent, in fixed order: (a) decision queue with risk rank and one-line asks, (b) not-ready list with reasons, (c) collision flags, (d) last week's outcomes with an honest success/rollback/unverified count, (e) recurring themes worth a policy tweak (e.g. the same client bounced three windows — their calendar needs documenting).
7. Deliver in chat for the organizer; on request post a plain-text copy as a note on the desk's CAB/agenda ticket (add_ticket_note).

## Guardrails

- The pack informs decisions; it makes none. No change is marked approved because it appeared in a brief.
- Never promote an unassessed or prerequisites-incomplete change into the decision queue to make the pack look fuller — "not ready" is a legitimate and useful section.
- Outcome reporting uses evidence standards, not optimism: unverified completions are reported as unverified, and rollbacks are never euphemized into "partial success."
- Aggregate honestly under result caps — if a board search may be capped, state the pack may be incomplete.
- Sanitized for the room: the pack names systems and clients only as the desk's tickets do; no credentials or personal data ride along.
