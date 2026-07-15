---
name: Autotask Ticket Categories
description: For desks synced to Autotask — classify tickets with Autotask's Issue Type → Sub-Issue Type pairs (plus ticket category/type) using only configured values; never invent classification values.
category: PSA-Specific
tools: [search_tickets, update_ticket, add_ticket_note]
---

# Autotask Ticket Categories

Applies to: **Autotask (Datto/Kaseya)**. Autotask classifies tickets on **Issue Type → Sub-Issue Type** (a two-level, globally configured taxonomy where each sub-issue belongs to exactly one issue type), plus Ticket Category and Ticket Type (Incident / Service Request / Problem / Change / Alert). Clean classification drives Autotask reporting, workflow rules, and contract routing — and invalid values fail or mangle the sync.

## When to use

- Classifying a new ticket on an Autotask desk ("set the issue type").
- Closure gates requiring issue/sub-issue before completion.
- Auditing recent tickets for defaulted or missing classification.

## Steps

1. Re-fetch the ticket with `search_tickets` and read the full thread — classify from the request body, never the title alone (see `skills/triage-and-routing/ticket-triage`).
2. Establish the valid value set. Thread does not expose Autotask's setup tables; derive values from: the desk's documented taxonomy (see `skills/psa-specific/psa-field-mapping-doc`), or values observed on recently closed tickets via `search_tickets`. Never use a value you have not seen configured for this tenant.
3. Pick Issue Type first, then a Sub-Issue Type **that belongs to that issue type** — sub-issues are parented in Autotask, so a mismatched pair is invalid even if both values exist.
4. Set Ticket Type by nature of the work: Incident (something broken), Service Request (something wanted), Problem (root cause of recurring incidents — see `skills/escalation/problem-ticket-creation`), Change, Alert (monitoring-sourced). Do not leave monitoring alerts typed as Incidents if the desk separates them.
5. If nothing fits confidently, say so and leave it for a human rather than defaulting to a catch-all — catch-alls make Autotask reporting quietly useless.
6. Propose the full classification (issue, sub-issue, ticket type, category if used) with one-line justification; apply with `update_ticket` after confirmation. Record non-obvious calls in a plain-text `add_ticket_note`.

## Guardrails

- **Never invent values**, and never pair a sub-issue with an issue type you have not seen it under.
- **Sync lag:** re-fetch full ticket detail before writing — classification, queue, or completion may have changed Autotask-side.
- Multi-issue tickets get split (`skills/triage-and-routing/multi-issue-ticket-splitter`), not classified by whichever issue was mentioned first.
- Do not reclassify closed tickets in bulk without confirmation — Autotask reporting for past periods may already have been consumed.
- Classification is global in Autotask (not per-queue); a queue move never requires reclassification — if someone reclassifies on every move, flag the habit.

## Cross-references

- CW equivalent (board-scoped, three-level): `skills/psa-specific/cw-type-subtype-item`
- Generic tree: `skills/triage-and-routing/intake-classification-tree`
