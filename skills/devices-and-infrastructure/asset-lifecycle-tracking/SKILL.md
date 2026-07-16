---
name: Asset Lifecycle Tracking
description: Keep asset status honest across its whole life — procurement, deployed, in-stock/spare, in-repair, refresh-due, retired — by reconciling RMM, documentation, and Liongard so no device is orphaned or wrongly counted. Use when someone asks to audit asset status, clean up the CMDB, or "what state are our assets actually in".
category: Devices & Infrastructure
tools: [search_ninjaone_devices, get_ninjaone_device, liongard_launchpoint, liongard_metric, search_itglue, create_ticket, add_ticket_note]
---

# Asset Lifecycle Tracking

Make the asset register tell the truth. Reconcile what the RMM sees, what documentation claims, and what Liongard reads, then correct each asset's lifecycle status so procurement, deployment, refresh, and retirement decisions rest on real state.

## When to use

- "Audit our asset statuses / clean up the CMDB."
- Assets showing as deployed that stopped checking in (or vice versa).
- Before a refresh or budget cycle, to know what's actually in each lifecycle stage.
- After onboarding/offboarding waves, to catch orphaned or mis-stated assets.

## Steps

1. Define scope (a client, a site, or a device class) and pull the three views: the RMM inventory (`search_ninjaone_devices`, `get_ninjaone_device` for last-contact and state), the documented asset register (`search_itglue`), and, where Liongard runs, device/endpoint inspectors (`liongard_launchpoint`, `liongard_metric`) as a third signal. State each source's freshness (last contact / dataprint age).
2. Reconcile across sources and classify each asset into a lifecycle stage:
   - Procurement/on-order (documented but not yet seen).
   - Deployed & checking in (in RMM, recent contact, has an assigned user/site).
   - In-stock / spare (documented, not assigned, not expected to check in).
   - In-repair / loaner.
   - Refresh-due (past its refresh window — cross-reference the hardware-refresh-forecast and warranty-eol-report skills).
   - Retired (should be gone — cross-reference the server-decommission-runbook for servers).
3. Flag the discrepancies that matter: in RMM but not documented (untracked asset), documented as deployed but not checking in for an extended period (possible lost/decommissioned-but-not-recorded — cross-reference rmm-cross-tool-reconciliation), assigned to a departed user, or marked retired yet still checking in (a wipe/removal that never happened).
4. For each discrepancy, propose the specific status correction and its owner — do not silently "fix" the register; corrections to the source of truth are a human action.
5. Output a reconciliation report: counts per lifecycle stage, the discrepancy list with proposed corrections, and the assets needing action first (untracked, orphaned, or retired-but-live). Post to the ticket as a plain-text note (`add_ticket_note`) or `create_ticket` to track cleanup.

## Guardrails

- This skill reconciles and recommends; it does not edit the asset register or RMM records. Status corrections are applied by a human in the source system.
- "Not checking in" is a signal, not a verdict — a spare or powered-off device is legitimately silent. State the last-contact age and let the owner judge before retiring anything.
- Trust each source only per its freshness; report last-contact and dataprint ages, and degrade gracefully when Liongard or documentation is absent (say the reconciliation is two-source, not three).
- If any inventory listing may have hit a result cap, report counts as "at least N".
- Notes posted to tickets are plain text — no markdown, no emojis.
