---
name: As-Built Documentation
description: Capture the as-built record at the end of a project — what was actually deployed, how it was configured, and how it differs from the design — so the delivered environment is documented while the details are still fresh, not reconstructed months later.
category: Documentation
tools: [search_clients, search_tickets, search_itglue, search_hudu, search_knowledge_base, notion-search, notion-create-pages, add_ticket_note]
---

# As-Built Documentation

Defines the as-built capture standard and produces the template to record what a completed project actually delivered — the real, final configuration, including where it diverged from the original plan. As-built is captured at project close while the engineer still remembers the decisions, and it becomes the environment's source of truth going forward.

## When to use

- A project (server migration, firewall replacement, new site build, cloud tenant setup, network refresh) just wrapped and the delivered state needs documenting.
- "Write up the as-built for the <project> we just finished for <client>."
- A project closed without as-built docs and the desk is now supporting an environment nobody documented.
- Defining the as-built standard so every project closes with consistent documentation.

## Steps

1. Establish the client and project with `search_clients` and `search_tickets` — pull the project ticket(s), the design/scope of work, change notes, and the closeout thread. The delta between the original design and what shipped is the most valuable part of an as-built, so anchor on both.
2. Check for existing documentation this as-built should update rather than duplicate: `search_itglue` / `search_hudu` (where enabled), `search_knowledge_base`, `notion-search` (if connected).
3. Produce the **as-built record** to this standard, populated only from project evidence:
   - **Project summary** — what was delivered, for which sites/systems, and the completion date.
   - **Final architecture** — the delivered topology/configuration (link the updated diagram; refresh it via documentation/network-documentation-standard if the change touched the network).
   - **Configuration detail** — the actual settings, IPs/hostnames, versions, roles, and integrations as deployed. Credentials by vault reference only.
   - **Deviations from design** — every place the delivered state differs from the original plan, with the reason (constraint, client decision, discovered condition). This is the section that saves the next engineer.
   - **Decommissioned / replaced** — what the project removed or superseded, so old records get retired.
   - **Known limitations & follow-ups** — anything deferred, worked-around, or left for a later phase.
   - **Handover** — who owns ongoing support, warranty/license impacts (feed documentation/warranty-license-registry), and any new runbooks needed (documentation/onboarding-runbook-per-client / offboarding).
4. Set the **filing discipline**: the as-built replaces or updates the client's environment documentation in the desk's platform under the client's section, dated with the project completion date, superseding the pre-project state. Confirm the location convention rather than inventing one.
5. Output the as-built as a paste-ready document plus a short list of downstream docs to update (diagram, registries, runbooks). If the desk keeps as-builts in Notion and the destination is confirmed, create with `notion-create-pages`; otherwise paste-ready blocks. Note the source project ticket for the reviewer, and attach the record to the project ticket via `add_ticket_note` if requested.

## Guardrails

- **As delivered, not as planned or as imagined.** Every configuration detail must trace to project evidence (ticket, change note, engineer confirmation). Do not carry forward design-doc values that were changed during delivery, and do not invent settings — unverified detail stays "to confirm."
- **No credentials in the as-built** — vault them and reference by entry title only (see documentation/credential-storage-audit).
- The as-built supersedes prior environment docs — flag the old records to retire so two "truths" don't coexist.
- Notes and documents are plain text per the Note Format Standard (documentation/note-format-standard) — they may sync to a PSA.
- Docs tools are search-only; writes go to Notion only when the member has connected it and confirmed the destination, otherwise paste-ready output. Degradation: without IT Glue/Hudu, build from the project tickets and engineer input and note that pre-existing docs could not be cross-checked.
