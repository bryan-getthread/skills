---
name: Priority Downgrade Guard
description: On a priority change, if AI triage lowered a priority that a human or the client had explicitly set higher, restore the higher priority — deterministic, restore-only, never an upgrade of its own. Answers the commonly requested "stop the auto-triage from quietly downgrading our urgent tickets" partner workflow.
category: Triage & Routing
tools: [search_tickets, update_ticket, list_ticket_priorities, add_ticket_note]
---

# Priority Downgrade Guard

Auto-prioritization occasionally lowers a priority that a person deliberately raised — burying an urgent ticket the client or a tech flagged. This skill watches priority changes and, when an automated downgrade overrode a human's explicit higher setting, puts the higher priority back. It only ever restores; it never raises a priority on its own judgment.

## When to use

- Auto-triage sometimes downgrades tickets a client or tech marked urgent.
- "Don't let the AI lower a priority a human set."
- Protecting VIP/critical flags from being overwritten by automated categorization.

## Steps

1. Read the ticket with `search_tickets`: the current priority, the prior priority, and the history of what changed it — specifically whether the drop was made by automated triage/auto-prioritization vs. a person.
2. Confirm two things from evidence: (a) the higher priority was set by a **human or the client explicitly** (a person's action or a clear client statement of urgency in the thread), and (b) the downgrade was made by **automation**, not a human deliberately lowering it.
3. If both hold, restore the human-set higher priority with `update_ticket` (confirm the priority value exists via `list_ticket_priorities`).
4. Post a plain-text note via `add_ticket_note`: what the human-set priority was, that automation downgraded it, and that it was restored.
5. In every other case, do nothing.

## Guardrails

- **Restore-only, deterministic.** This skill never sets a priority higher than the human previously set, and never raises priority on its own assessment. Its only move is putting back a human-set level that automation lowered.
- If a **human** lowered the priority, leave it — people are allowed to downgrade; only automated downgrades are reversed.
- If it's unclear whether the higher priority was human-set or whether automation did the downgrade, do nothing.
- Priority restore + one note are the only writes. No status/owner/board changes.
- Notes plain text (PSA compatibility).

## Unattended (Flows) variant

- Fires on the **priority-change event** — a supported ticket condition.
- Your entire reply is the note, verbatim: `PRIORITY RESTORED to <level> (human-set; downgraded by automation).` or `NO ACTION.`
- Deterministic stops: higher priority not clearly human/client-set → no action; downgrade made by a human → no action; ambiguous provenance → no action.
- Restore at most once per downgrade event; never escalate beyond the prior human-set level.
