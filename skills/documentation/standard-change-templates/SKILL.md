---
name: Standard Change Templates
description: Build the library of pre-approved standard-change definitions — the low-risk, repeatable changes that skip full CAB review — each with its scope, procedure, backout, and validation, so routine changes are executed consistently and safely.
category: Documentation
tools: [search_tickets, search_knowledge_base, search_itglue, search_hudu, notion-search, notion-create-pages, notion-update-page, add_ticket_note]
---

# Standard Change Templates

Defines and documents the desk's library of **standard changes** — the repeatable, low-risk changes (routine firewall rule add, standard software deploy, disk expansion, certificate renewal, standard user-access change) that have been pre-approved so they don't need per-instance CAB approval. Each template fixes the scope, steps, backout, and validation so the change runs the same way every time. This is the documentation of the definitions; running the change-management process around them is change-and-problem-management.

## When to use

- "Let's define our standard changes so routine work stops going through full change approval."
- Recurring change tickets are being raised ad hoc and the desk wants a consistent, pre-approved template.
- A change type has proven low-risk and repeatable enough to promote to standard (feed from change-and-problem-management/change-risk-assessment).
- Reviewing the standard-change library to confirm each definition is still valid and low-risk.

## Steps

1. Identify candidate standard changes from evidence: `search_tickets` for change types that recur, are low-risk, and follow the same steps each time; `search_knowledge_base`, `search_itglue` / `search_hudu` (where enabled), and `notion-search` (if connected) for existing change procedures and any current standard-change list. A change only qualifies as "standard" if it is repeatable, low-risk, and has a proven procedure — flag anything that doesn't clearly meet that bar for full change review instead (change-and-problem-management/change-request-intake).
2. For each qualifying change, write the **standard-change template**:
   - **Name & description** — what the change is and the outcome.
   - **Applicability / scope** — exactly which situations this template covers, and the boundaries beyond which it stops being standard and needs full CAB (change-and-problem-management/cab-brief-builder). Being explicit about the boundary is what keeps a standard change safe.
   - **Pre-approval basis** — why this is pre-approved (risk class, authority who approved the template, review date).
   - **Prerequisites** — access, maintenance window, notifications, prior checks.
   - **Procedure** — numbered, imperative, independently verifiable steps.
   - **Validation** — how to confirm success.
   - **Backout plan** — how to reverse it if validation fails.
   - **Records to update** — CMDB/asset, config-change log (documentation/config-change-log), and any as-built impact.
3. Note the **governance** on the template: who owns it, its review cadence, and that promoting a new change to standard (or retiring one) is an authorized decision, not an automatic one.
4. Set the **storage discipline**: templates live in the desk's change/standard-change library in its documentation platform, versioned and dated. Confirm the location convention rather than inventing one.
5. Output the template(s) as paste-ready documents plus the list of candidates that did NOT qualify (with why). If the desk keeps the library in Notion and the destination is confirmed, create with `notion-create-pages` or refresh with `notion-update-page`; otherwise paste-ready blocks. Attach a template to its governing ticket via `add_ticket_note` if requested.

## Guardrails

- **Only genuinely low-risk, repeatable, proven changes become standard templates.** When risk or repeatability is unclear, route to full change review — do not lower the bar to fit more changes into the pre-approved lane.
- **The template's scope boundary is the safety mechanism** — every template must state where it stops applying and full CAB takes over. A standard change used outside its scope is no longer pre-approved.
- Procedures are based on what tickets actually show worked; do not invent steps, backout plans, or approval authorities. Pre-approval must reference a real authorizing decision.
- Templates are definitions for humans to execute — this skill does not approve, promote, or run changes.
- **No credentials in templates** — reference the vault by entry title only (see documentation/credential-storage-audit).
- Notes and templates are plain text per the Note Format Standard (documentation/note-format-standard) — they may sync to a PSA.
- Docs tools are search-only; writes go to Notion only when the member has connected it and confirmed the destination, otherwise paste-ready output. Degradation: without IT Glue/Hudu, source candidates from ticket history and the KB and note existing procedures may be missed.
