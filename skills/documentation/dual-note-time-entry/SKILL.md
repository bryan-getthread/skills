---
name: Dual-Note Time Entry
description: One command logs the external client-facing time-entry summary and adds a paired internal note for the same work — the Autotask-style paired-note pattern in a single step. Answers the commonly requested "external summary plus internal note together" workflow; runs manually on demand.
category: Documentation
tools: [log_time_entry, add_ticket_note, run_assistive_ai]
---

# Dual-Note Time Entry

Some desks (Autotask muscle-memory especially) want every piece of work recorded twice: a clean external summary on the time entry the client sees, and a candid internal note only the team sees. This does both from one prompt, tied to the same work.

## When to use

- "Log my time and drop an internal note about what really happened"
- "External summary for the client, internal note for the team — same work"
- "Autotask-style paired notes for this ticket"
- Any request that names both a client-facing time entry and a separate internal note for one session

## Steps

1. Identify the work session being recorded (the active timer window, or what the member describes). Draft two texts from the same underlying facts:
   - **External (time entry):** what the client should read — outcome-focused, professional, plain-text, no internal blame or speculation.
   - **Internal (note):** the candid version — root cause, workarounds, "watch this", follow-ups — for the team only.
2. Use run_assistive_ai to draft both if the member wants them generated; otherwise format what they dictate. Keep both plain-text and PSA-safe (no markdown/emojis).
3. Preview both texts side by side, clearly labeled External vs Internal, plus the date and any duration. Wait for an explicit yes. Let the member edit either independently.
4. On confirm, call log_time_entry with the external summary, then add_ticket_note with the internal note, and make the internal note visibly reference the same work (e.g. same date/session) so the pairing is obvious later.
5. Report both writes back: "Logged time entry (external) + added internal note for <date> session."

## Guardrails

- Confirm before writing. Both the time entry and the note wait on one explicit go-ahead over the previewed text.
- Keep the two audiences separate: nothing blame-y, speculative, or internal-only may leak into the external time entry; nothing gets silently dropped from the internal note to soften it.
- Only record work the ticket/session actually evidences. Do not invent duration or steps; if duration is unknown and not supplied, ask.
- Plain-text both outputs for PSA compatibility.
- If the member only wants one of the two, don't force the pair — log just the time entry (that's the Custom Time Entry Writer skill's job) and skip the note.
- Member-invoked only. Flows fire on ticket events and cannot schedule time-entry writing, so there is no unattended variant.
