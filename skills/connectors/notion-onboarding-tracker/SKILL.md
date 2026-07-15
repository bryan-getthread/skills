---
name: Notion Onboarding Tracker
description: Run a new-hire progress tracker in Notion — read where the trainee is, update checklist items and quiz results, and answer "how is <new hire> doing". The trainer pattern: Notion holds state, Super Magic reads and writes it across sessions. Use for "set up an onboarding tracker", "mark <trainee>'s phase 2 complete", or "how far along is our new tech".
category: Connectors
tools: [notion-search, notion-fetch, notion-create-pages, notion-update-page, notion-query-data-sources, notion-create-comment, notion-create-database]
---

# Notion Onboarding Tracker

Persistent training state that survives between chat sessions: a per-trainee Notion page (or database row) holding phases, checklist items, quiz scores, and trainer notes — read at the start of a coaching session, updated at the end.

## When to use

- "Create an onboarding tracker for our new L1 starting Monday."
- Mid-training: "Quiz <trainee> on phase 2 and record the score." / "Mark the ticketing-basics module done."
- Trainer check-in: "How is <trainee> progressing? What's left?"

## Steps

1. **Locate state first, always.** `notion-search` / `notion-query-data-sources` for the trainee's tracker page and `notion-fetch` it before answering any progress question or running any coaching — never coach from memory of a previous session.
2. **Setup (once per program):** if no tracker exists, propose a database with: Trainee, Phase, per-module checklist (as sub-items or a related database), Quiz scores, Trainer notes, Started/Target dates. Confirm schema + teamspace, then create. Seed the module list from the member's curriculum — don't invent one; if none exists, offer a draft curriculum clearly labeled as a proposal.
3. **Session loop (repeatable):** read current state → run the session's activity (walk a module, quiz the trainee against the rubric on the page, review a practice ticket) → summarize what happened → on confirmation, `notion-update-page` with completed items, scores, and a dated trainer note.
4. **Progress reporting:** answer "how are they doing" from tracker data only: phase, completed vs remaining modules, quiz trend, days in program vs target. Flag stalls (no progress in N days) neutrally.
5. Use `notion-create-comment` to @mention the trainer when something needs their attention (a failed quiz retake, a blocked module).

## Guardrails

- **Member-authenticated connector:** requires the member's Notion connection; degrade by keeping session results in chat and telling the member to record them, noting state will not persist.
- Read-before-write every session; two trainers updating one tracker means your cached picture may be stale.
- Quiz scoring uses the rubric stored on the tracker; if there is no rubric, grade descriptively and say the grading is unrubriced.
- Progress notes describe observed work ("completed module 3; quiz 7/10"), never personality judgments — this record may surface in HR contexts.
- Trainee-facing encouragement stays in chat; the tracker gets facts.
