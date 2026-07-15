---
name: Stale Device Cleanup
description: Clean up stale Entra device objects on a last-activity threshold — with the BitLocker-key-loss warning, Autopilot exclusions, and a disable-first pattern before any delete. Use for "there are hundreds of old devices in Entra", periodic device hygiene, or pre-migration directory cleanup.
category: M365 Administration
tools: [search_tickets, search_knowledge_base, search_itglue, search_hudu, add_ticket_note, update_ticket, send_approval, schedule_ticket, web_search]
---

# Stale Device Cleanup

Deleting a device object is the rare hygiene task that can destroy data
recovery: BitLocker keys stored on the Entra device object die with it. This
skill sequences the cleanup so keys are preserved first, Autopilot devices
are excluded, and nothing is deleted that was merely asleep.

## When to use

- "Entra has <hundreds of> devices that haven't checked in for a year."
- Periodic device-hygiene pass for a managed tenant.
- Device-count-based licensing or reporting is skewed by dead records.
- Pre-cleanup before a tenant migration or management-tool change.

## Steps

1. **Inventory on activity.** Tech exports Entra devices with approximate
   last sign-in timestamp, join type, management state, and registration
   date. Choose the staleness threshold from the client's standard
   (search_itglue / search_hudu / search_knowledge_base) or default to 90
   days minimum — Microsoft's own guidance floor; 180 days is the
   conservative default for deletes. Date the export.
2. **Build the exclusion list before the candidate list:**
   - **Autopilot-registered devices** — do not delete the Entra object of a
     device with an Autopilot registration; deregister from Autopilot first
     if the hardware is truly retired (see autopilot-deployment). Otherwise
     redeployment breaks.
   - **Seasonal/spare/loaner patterns** — devices offline by design
     (classroom carts, field spares, seasonal staff). Ask the client which
     patterns exist rather than guessing.
   - **Recently imaged or in-transit devices** — young registration date with
     no activity is a device in a box, not a stale one.
3. **Preserve BitLocker keys FIRST.** Before disabling or deleting anything:
   tech exports/backs up BitLocker recovery keys for every candidate device
   to the client's documented secure store. Deleting the device object
   deletes its stored recovery keys — if the physical disk ever surfaces
   needing recovery, that data is gone. This step is complete before any
   candidate moves to step 4, and the note records where keys were preserved
   (location reference, never the keys themselves).
4. **Disable first, delete later.** Disable the candidate devices (blocks
   authentication) and wait an agreed window — 30 days is a sane default.
   A disabled device that was actually alive fails loudly and reversibly;
   a deleted one does not. After the window with no breakage reports, delete.
   Schedule the deletion pass (schedule_ticket) so the window is real.
5. **Approval gate.** send_approval to the client's documented authority
   before the disable pass: threshold used, candidate count, exclusions
   applied, BitLocker preservation confirmation, the disable→wait→delete
   schedule, and rollback (re-enable during the window; post-delete recovery
   is re-enrollment, and hybrid-joined devices may need on-prem cleanup and
   re-sync — say so).
6. **Hybrid honesty.** For hybrid-joined devices, the on-prem AD computer
   object is the source: deleting only the Entra object gets re-created by
   sync. Stale hybrid devices are cleaned in AD (client's on-prem change
   process) and allowed to age out of Entra — flag these as a separate
   workstream rather than fighting the sync.
7. **Document.** Plain-text note: threshold, counts (candidates, excluded,
   disabled, deleted — dated), key-preservation reference, approver,
   schedule, and the recurring hygiene cadence proposed (schedule_ticket).

## Guardrails

- No device deletion, ever, without BitLocker key preservation confirmed for
  the batch — this is the one irreversible data loss in the whole workflow.
- Autopilot-registered devices are excluded from deletion until explicitly
  deregistered as retired hardware.
- Disable-first is mandatory; straight-to-delete is only acceptable for
  never-activated duplicate records, and the note must say why.
- Intune-managed state and Entra object state differ — cleaning the Entra
  object of an actively managed device breaks it; the inventory must join
  both views before naming candidates.
- Counts are point-in-time; re-pull the export if more than two weeks pass
  between approval and execution.
