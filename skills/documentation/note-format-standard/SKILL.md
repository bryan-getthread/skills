---
name: Note Format Standard
description: Base skill defining the house format for internal ticket notes — structure, tone, and the plain-text rules for notes that sync to a PSA. Other skills reference this standard.
category: Documentation
tools: [add_ticket_note]
---

# Note Format Standard

The shared formatting standard every note-writing skill follows. Load alongside skills like
ticket-summary-and-closure-note, time-entry-cleanup, or runbook-recommender to make all
notes on the desk look like they came from one team.

## When to use

- Loaded as a companion whenever another skill writes or drafts a ticket note.
- "Format this as a proper internal note."
- A desk wants consistent note structure across techs and automations.

## Steps

1. Structure every internal note in this fixed order (omit empty sections, keep the order):
   1. **Summary** — one line: what this note records.
   2. **Details** — what was observed/done/decided, past tense, factual.
   3. **Next step** — single owner-and-action line, if any.
   4. **Source** — where the information came from (call, email, alert, meeting) when not obvious.
2. Apply the tone rules: professional, plain language, no blame, no speculation presented as fact ("suspected:" prefix for hypotheses), no internal jokes — notes outlive the moment and may be read by the client's auditors.
3. Apply the **PSA plain-text rules** whenever a note syncs to a PSA (ConnectWise, Autotask, Halo — assume yes unless the desk confirms otherwise):
   - Plain text only: no markdown syntax (`**`, `#`, backticks, tables), no emojis, no smart quotes, no em-dashes (use a hyphen or comma).
   - Structure with CAPITALIZED single-word labels and hyphen lists:
     `SUMMARY: ...` / `DETAILS:` / `- item` / `NEXT STEP: ...`
   - Blank line between sections; no line longer than ~100 characters where the source PSA wraps badly.
4. Internal vs client-visible: this standard governs **internal** notes. Anything client-visible follows the desk's client-communication voice instead — never mix the two in one note.
5. When another skill produces a note, reformat its content to this standard without changing its meaning, then post via `add_ticket_note` only when that skill's own confirmation rules are satisfied.

## Guardrails

- This skill defines format; it never adds, removes, or reinterprets content — meaning is owned by the calling skill or technician.
- Plain-text rules win over prettiness: when unsure whether a note syncs to a PSA, write plain text.
- No credentials, one-time codes, or personal data beyond what the workflow requires in any note.
- Desk-specific overrides (their own labels, language, banned words) take precedence over the defaults here; keep the section order unless the desk overrides that too.
- Non-English desks: same structure, labels translated to the desk's working language.
