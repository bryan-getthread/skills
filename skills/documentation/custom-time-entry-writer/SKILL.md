---
name: Custom Time Entry Writer
description: Pick a recap template or saved prompt and output language, summarize the thread scoped to the active timer window (since the last time entry), preview it, then log the time entry on confirm — including catching up multiple retroactive entries. Answers the commonly requested "write my time entry the way I want it" workflow; runs manually on demand.
category: Documentation
tools: [list_recap_templates, run_assistive_ai, log_time_entry, search_tickets]
---

# Custom Time Entry Writer

Turn the work on a ticket into a time entry that matches the desk's own template, in the language the member wants — scoped to just the window since the last entry, so nothing is double-counted and nothing retroactive is missed.

## When to use

- "Write my time entry for this ticket using our recap template"
- "Log time for what I did since my last entry"
- "I forgot to log the last three sessions — catch me up"
- "Summarize this in Spanish and log it"
- Any request to draft-and-log a time entry in a specific format or language

## Steps

1. Establish the window. Find the most recent existing time entry on the ticket and scope the summary to work **since that entry** (or since the active timer started). If there is no prior entry, scope to the whole thread. State the window you chose so the member can correct it.
2. Pick the format. Offer the desk's recap templates via list_recap_templates, or accept a saved prompt the member names. If they specify an output language, carry it through the whole entry.
3. Generate the summary with run_assistive_ai, recording only work the thread shows was actually done — no inferred steps, no filler. Keep it plain-text and PSA-safe (no markdown, no emojis) since it syncs to the PSA.
4. **Multiple retroactive entries:** if the window since the last entry contains several distinct work sessions on different days, offer to split them into one entry per session (with the right date each) rather than collapsing everything into a single lump. Present the proposed split for confirmation.
5. Preview every entry — summary text, date, and duration if the member supplied one — and wait for an explicit yes.
6. On confirm, call log_time_entry for each approved entry. Report back exactly what was logged. If the member changes wording, re-preview before writing.

## Guardrails

- Confirm before writing. Never call log_time_entry without an explicit go-ahead on the previewed text.
- Only summarize work the ticket actually evidences. Do not invent steps, durations, or outcomes; if duration wasn't provided and the timer doesn't supply it, ask rather than guess.
- Scope to the window since the last entry to avoid double-logging already-billed work; if the boundary is ambiguous, state your assumption and let the member correct it.
- Keep entry text plain-text for PSA compatibility.
- For non-English desks, generate directly in the requested language rather than translating a draft after the fact.
- This is a member-invoked skill. Flows fire on ticket events and cannot schedule or time-trigger time-entry writing, so there is no unattended variant.
