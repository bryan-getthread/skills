---
name: Follow-up Chaser
description: Draft a polite, escalating follow-up when a client hasn't replied on a ticket — first nudge, second nudge, or final pre-closure attempt, tone matched to the attempt number.
category: Communication
tools: [search_tickets, view_openDraft, add_ticket_note]
---

# Follow-up Chaser

Drafts the right follow-up for a ticket waiting on the client, with tone that escalates gently across attempts instead of repeating the same nudge three times.

## When to use

- "The client hasn't responded — draft a follow-up."
- "Second follow-up on this ticket."
- Working a stale-ticket queue and each waiting-on-client ticket needs its next chase.

## Steps

1. Read the ticket thread. Establish: what we asked the client for, when we last asked, and how many follow-up attempts are already documented. Count only real, recorded attempts.
2. Confirm the ball is genuinely in the client's court. If the last substantive action needed is actually ours (or a vendor's), say so and stop — chasing a client for our own stall is a relationship error.
3. Draft the attempt that comes next on the tone ladder, per the Email Baseline Standard skill (communication/email-baseline-standard):
   - **Attempt 1 — light nudge.** Friendly, assumes they're busy. Restate what we need in one sentence and why it unblocks their fix.
   - **Attempt 2 — direct ask.** Still warm, but explicit: what we need, that the ticket is paused waiting on it, and an easy out ("if this is no longer an issue, let us know and we'll close it out").
   - **Attempt 3 — closure warning.** Plain statement that without a reply by <specific date> we'll close the ticket, and that replying any time reopens it. This is the last chase before the three-strikes final email (communication/three-strikes-final-email).
4. Every attempt restates the original ask — never send a content-free "just checking in."
5. Present as an open draft via view_openDraft. Offer to add a plain-text internal note recording the attempt number and date via add_ticket_note.

## Guardrails

- Never skip rungs: don't draft attempt 3 tone when only one attempt is documented. The ladder count comes from the ticket record, not from vibes.
- Never bulk-send follow-ups across tickets without per-ticket human review.
- Dates in the closure warning must be real and honored — no "we'll close this Friday" unless someone will.
- Internal notes in plain text only (PSA compatibility).
- Degradation: view_openDraft is in-app only — over external MCP, output the draft in chat labeled with the attempt number.

## Unattended (Flows) variant

- Your entire reply is the follow-up email body for the correct attempt number — no narration.
- If the attempt count is ambiguous, or the ticket is not actually waiting on the client, output nothing.
