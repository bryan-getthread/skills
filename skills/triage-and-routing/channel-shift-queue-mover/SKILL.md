---
name: Channel Shift Queue Mover
description: When a client reply arrives on a channel that doesn't match the ticket's current board/queue (e.g. a chat reply on a mail-queue ticket), move the ticket to the board that matches the new channel. Answers the commonly requested "keep tickets on the queue that matches how the client is actually talking to us" partner workflow.
category: Triage & Routing
tools: [search_tickets, list_boards, update_ticket, add_ticket_note]
---

# Channel Shift Queue Mover

A ticket that started as email but is now a live chat conversation belongs on the chat queue, not buried in mail. This skill detects when a client reply's channel no longer matches the ticket's board and moves the ticket to the board that matches the current channel, per the desk's documented channel→board map.

## When to use

- Conversations that migrate channels (email → chat, messenger → mail) sit on the wrong queue.
- "If they reply in chat, move it to the chat board."
- Keeping live-chat conversations off the slower mail queue so they get real-time handling.

## Steps

1. Read the reply and the ticket with `search_tickets`: the reply's channel/source and the ticket's current board.
2. Look up the desk's **documented channel→board mapping** (e.g. "chat replies belong on the chat board"; `list_boards` to confirm targets). If the reply's channel already matches the ticket's board, do nothing.
3. If the channel maps to a different board than the ticket's current one, move it with `update_ticket` to the mapped board.
4. Post a plain-text note via `add_ticket_note`: the reply channel detected, the board moved from and to.

## Guardrails

- Move only on a clear channel mismatch backed by the documented mapping — never guess a target board.
- Preserve everything else: this skill changes the board only. Do not reset status, owner, priority, or classification (moving boards may re-scope classification — flag that in the note if so, per `skills/psa-specific/cw-type-subtype-item`).
- Do not thrash: if the ticket was moved by this rule very recently and the client is alternating channels, note the churn and leave it for a human rather than bouncing it repeatedly.
- Board change + one note are the only writes.
- Notes plain text (PSA compatibility).

## Unattended (Flows) variant

- Fires on the **reply event**, using the flow's **source / inbox / messenger touchpoint** conditions — supported filter attributes — with a **set board** action available natively.
- Your entire reply is the note, verbatim: `QUEUE MOVED. Reply channel: <channel>. Board: <from> → <to>.` or `NO ACTION.`
- Deterministic stops: channel already matches board → no action; no mapped target board → no action; recent repeated move (channel thrash) → no action, note for human.
- Change board only; never touch status/owner/priority in this variant.
