---
name: Asset & CMDB Hygiene
description: Enforce ticket-to-asset linkage discipline — every device ticket names the device, and every "unknown asset" ticket becomes an inventory fix — so the asset record converges on reality instead of drifting from it.
category: Change & Problem Management
tools: [search_tickets, add_ticket_note, update_ticket, search_ninjaone_devices, get_ninjaone_device, search_itglue, search_hudu, create_ticket]
---

# Asset & CMDB Hygiene

The asset record (RMM inventory, IT Glue/Hudu configurations) decays unless the ticket flow feeds it: tickets that name their asset keep history attributable, and tickets about devices the inventory doesn't know are free discovery — each one is an inventory gap announcing itself. This skill enforces the linkage and converts the misses into fixes.

## When to use

- "Which recent tickets aren't linked to an asset?" — the periodic linkage sweep.
- A ticket mentions a device the RMM/documentation doesn't recognize ("unknown asset" moment).
- Before an asset-based report (warranty/EOL, patch compliance, hardware refresh) whose accuracy depends on linkage.
- After onboarding or a site change, when inventory drift is most likely.

## Steps

1. **Linkage sweep**: search_tickets over the window (default 30 days) for device-implicating tickets — hardware faults, performance, patching, peripheral and connectivity issues — and check each for an asset reference (configuration link, hostname, serial, or asset tag in the body/notes). Split searches per board; note result caps. Bucket into: linked, unlinked-but-identifiable, and unlinked-unidentifiable.
2. **Unlinked-but-identifiable**: the ticket names a hostname/serial/user-device pairing — corroborate against the RMM (search_ninjaone_devices / get_ninjaone_device where enabled) and documentation (search_itglue / search_hudu where enabled). Exact identifier match → record the linkage: plain-text note on the ticket naming the asset (hostname + serial), and set the configuration/asset field where the desk uses one (update_ticket). Name-similarity alone is not a match — same rule as contact remapping: low confidence, no write.
3. **Unknown assets — the valuable failures**: the ticket describes a real device the inventory doesn't have (or has wrong: retired-but-active, reassigned, wrong site). Don't shrug these off ticket-by-ticket — each one becomes an inventory-fix ticket (create_ticket): what the desk ticket showed, what the inventory says, the specific correction needed (enroll in RMM / update documentation / confirm retirement). Cross-link both ways. This is the loop that makes the CMDB converge on reality.
4. **Reconciliation triggers**: when the sweep shows clusters — several unknown assets at one client/site — recommend a full inventory reconciliation pass there (rmm-cross-tool-reconciliation in devices-and-infrastructure does the RMM-vs-RMM/documentation diff; this skill feeds it the ticket-side evidence of where drift is worst).
5. **Report**: linkage rate over the window (linked / total device-implicating, with cap caveats), unknown-asset tickets found and inventory fixes opened, repeat offenders (boards or techs whose tickets consistently skip asset references — a habit fix, routed to the lead as coaching data, not blame), and clients whose drift cluster warrants reconciliation.
6. Where the desk wants it standing: recommend the intake-side habit ("device tickets carry hostname or serial before leaving triage") be added to the desk's intake standard (ticket-intake-formatting in documentation) — enforcement at intake beats cleanup after.

## Guardrails

- Never link on name similarity or guesswork — a wrong asset linkage poisons device history worse than a missing one. Low confidence → flag for a human, no write.
- Inventory fixes are proposed as tickets for humans to execute; the agent does not create, retire, or modify inventory/CMDB records itself.
- Degrade gracefully: with no RMM or documentation integration enabled for the tenant, the skill still runs the linkage sweep on ticket text alone and says which corroboration steps were unavailable.
- Ticket data proves a device was discussed, not that it exists as described — unknown-asset fix tickets ask for verification, they don't assert inventory truth.
- Result-cap honesty: linkage rates from capped searches are labeled as sampled, not census.
