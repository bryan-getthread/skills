---
name: Voice Commitment Tracker
description: Sweep commitments made on calls — "we'll call you back by...", "a tech will be onsite tomorrow" — against what actually happened, and flag every overdue or unrecorded promise. Use for "did we keep our callbacks" or a daily commitment audit.
category: Voice & Messenger
tools: [search_tickets, add_ticket_note, schedule_ticket, update_ticket]
---

# Voice Commitment Tracker

Calls generate promises that email never sees: the AI or a tech says "someone will call you back within the hour," and unless that lands on the ticket and gets done, the client's next call is angry. This skill sweeps call-sourced commitments against reality and flags the debt.

## When to use

- "Which callbacks are we behind on?"
- Daily sweep of voice-sourced tickets for overdue or missing commitments.
- "The AI told this caller someone would ring back — did anyone?"
- Follow-through check after After-Hours Voicemail Digest hands the morning callback list to the desk.

## Steps

1. Collect candidates with `search_tickets`: voice-sourced tickets updated in the window (default: last 3 business days) that are still open, plus any explicitly given. Note result-cap exposure per search.
2. For each ticket, extract commitments from two places:
   a. **Structured** — `CALLBACK OWED:` lines and intake-note commitment lists (from Voicemail to Ticket / Voice Transcript Intake),
   b. **Raw transcript** — promise phrases the notes missed: "call you back by", "we'll have someone", "I'll email you", "onsite tomorrow", "escalating this today". A promise in the transcript with no ticket artifact is itself a finding (the commitment was never recorded).
3. For each commitment, establish who owes it, what was promised, and the deadline — stated exactly, or the desk's standard SLA labeled as such.
4. Check reality against the ticket record: a later note, time entry, outbound message, or schedule entry that plausibly fulfills the promise. Only quotable evidence counts — an assignment is not a callback.
5. Classify: **kept** (evidence found), **due soon** (deadline ahead), **overdue** (deadline passed, no evidence), **unrecorded** (promised on the call, never captured). Do not guess "probably called" — no evidence means not kept, flagged as "no record of fulfillment" rather than an accusation.
6. Report ordered by damage: overdue first (oldest breach first), then unrecorded, then due-soon. Each line: ticket, client, the promise verbatim, deadline, owner, evidence status.
7. On confirmation: post a plain-text flag note on overdue tickets via `add_ticket_note` (`COMMITMENT OVERDUE: <promise> — due <time>, no fulfillment recorded.`), and offer `schedule_ticket` to book the make-good callback for the owner.

## Guardrails

- Commitments are verbatim quotes with a source (note or transcript line). Never paraphrase a promise into something bigger or smaller than what was said.
- "No record of fulfillment" is the strongest claim this skill makes — the tech may have called from a cell without logging it. The flag asks for confirmation, it does not indict.
- This skill never contacts the client and never marks a commitment fulfilled; humans (or their logged actions) do that.
- Do not create schedule entries or notes without confirmation in attended use.
- Vague promises ("we'll look into it") are tracked without a deadline, reported in a separate "soft commitments" section — not manufactured into hard SLAs.
- Plain-text notes for PSA-synced tickets.
