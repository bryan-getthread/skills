---
name: Warranty & License Registry
description: Build and maintain a per-client registry of hardware warranties and software licenses with renewal dates — so expiries surface before they lapse instead of being discovered when a failed device turns out to be out of warranty.
category: Documentation
tools: [search_clients, search_tickets, search_itglue, search_hudu, search_knowledge_base, notion-search, notion-create-pages, notion-update-page, add_ticket_note, create_ticket]
---

# Warranty & License Registry

Assembles the per-client list of what's under warranty, what's licensed, and when each expires — the record that turns "is this laptop still covered?" and "when does the antivirus renew?" into a lookup instead of a scramble. Sourced from real records; renewal dates are never guessed.

## When to use

- "Build a warranty and license registry for <client>."
- Onboarding a client and populating the warranties/licenses section of the doc pack (see documentation/client-onboarding-documentation-pack).
- Budget/renewal planning — the account owner needs upcoming expiries to forecast spend.
- A device failed and nobody knew whether it was in warranty; the desk wants that never to happen again.

## Steps

1. Establish the client with `search_clients`, then gather the inventory from records: `search_itglue` / `search_hudu` (where enabled) for asset and license records; a Liongard read where the partner runs the relevant inspector (verify the inspector exists and last ran successfully; state dataprint age) for software/config that implies licensing; `search_tickets` for hardware purchases, warranty claims, and license renewals in history; `notion-search` (if connected) and `search_knowledge_base` for existing registries.
2. Capture two registers, each row sourced (leave unknown fields blank-flagged, never guessed):
   - **Hardware warranties** — device/model, serial or asset tag, vendor, warranty type/level, **start and expiry date**, and coverage scope. The serial is the lookup key for a vendor warranty check.
   - **Software licenses** — product, edition, license/subscription count and assignment, license key **by reference** (vault entry title — not the key itself), vendor/reseller, and **renewal/expiry date**.
3. Compute the **renewal view**: sort by expiry date and flag anything expired, expiring within the client's lead-time window, or missing an expiry date entirely. Missing dates are their own finding — an unknown renewal is a risk, not a blank.
4. Set the **storage and cadence discipline**: the registry lives under the client's section in the desk's documentation platform, dated, reviewed on a set cadence and on every hardware purchase or license change. Confirm the location convention rather than inventing one. Because Flows cannot fire on elapsed time or dates-relative-to-now, treat renewal surfacing as a **manual review cadence** (or an external scheduler), not a Flow.
5. Output both registers plus the renewal view (expired / expiring-soon / missing-date). If the desk keeps registries in Notion and the destination is confirmed, create with `notion-create-pages` or refresh with `notion-update-page`; otherwise paste-ready blocks. For imminent or lapsed renewals, raise a renewal task via `create_ticket` (or `add_ticket_note` on an existing account ticket) if requested.

## Guardrails

- **Sourced facts only** — warranty and renewal dates, serials, and counts must trace to a record, vendor confirmation, or ticket. Never invent an expiry date; missing dates are flagged for capture.
- **License keys and credentials never live in the registry** — reference the vault entry by title only (see documentation/credential-storage-audit).
- Liongard-derived license inference requires the inspector to exist and have run successfully; state dataprint age and degrade to record/ticket sourcing when absent.
- Renewal alerting is a manual cadence, not a Flow — Flows have no scheduled or elapsed-time trigger. Do not promise automatic date-based reminders through Flows.
- Notes and tables are plain text per the Note Format Standard (documentation/note-format-standard) — they may sync to a PSA.
- Docs tools are search-only; writes go to Notion only when the member has connected it and confirmed the destination, otherwise paste-ready output. Degradation: without IT Glue/Hudu and Liongard, build from ticket history and requester knowledge and mark the registry partial.
