---
name: Handwritten Notes Structurer
description: Turn rough onsite or call notes — shorthand, fragments, dictation — into a structured internal note, with an optional time-entry draft.
category: Documentation
tools: [search_tickets, add_ticket_note, log_time_entry]
---

# Handwritten Notes Structurer

Convert the scribbles a tech brings back from an onsite visit or call into a proper
internal note the rest of the desk can act on — plus a time-entry draft if wanted.

## When to use

- "Here are my notes from the <client> onsite — write them up."
- A tech pastes phone-call shorthand ("usr says slow since Tue, chk dsk 90%, ordrd ssd?").
- Field-tech dictation or photo-transcribed notes need structuring before end of day.

## Steps

1. Take the raw notes as pasted. Use `search_tickets` only to resolve references — which ticket, which <user>, which <device> — never to add content the notes don't contain.
2. Decode the shorthand conservatively: expand obvious abbreviations; anything genuinely ambiguous becomes an explicit `[unclear: "ordrd ssd?" - ordered or recommending an SSD?]` question rather than a guess.
3. Structure the write-up per the house **note-format-standard** (load it if available):
   - **Summary** — one line on the visit/call outcome.
   - **Details** — observations and work performed, past tense, in the order it happened.
   - **Findings** — facts discovered about the environment (candidates for environment-facts-updater).
   - **Next step** — only what the notes explicitly commit to, with owner.
   - **Open questions** — every `[unclear]` item, for the tech to resolve.
4. Apply the zero-assumption rule throughout: recommendations stay recommendations, "check X" stays a plan unless the notes say it was done, and no verification is claimed that the notes do not state.
5. If asked, draft the **time entry**: work performed in clean past tense plus a duration — from the notes if stated, otherwise from the stated visit start/end; if neither exists, ask. Follow time-entry-cleanup's field order and banned-word replacements.
6. Present the structured note (and time-entry draft) for the tech's review. Post with `add_ticket_note` / `log_time_entry` only after they confirm — the open questions should be resolved or explicitly kept before posting.

## Guardrails

- The tech's meaning is sacred: restructure and clarify, never embellish, never resolve ambiguity by picking the flattering reading.
- No fabricated durations, serial numbers, or measurements — `[not recorded]` beats a plausible number.
- Client-facing wording is out of scope here; this produces internal notes (route through the desk's client-reply skill for anything client-visible).
- Confirm before writing: notes and time entries touch the record and billing; when the tech disappears mid-flow, leave everything as a draft in chat.
- Works in the desk's language — structure the note in the language the notes were written in unless asked to translate.
