---
name: Device Wipe Workflows
description: Choose the right Intune remote action — retire, wipe, fresh start, Autopilot reset, or delete — with explicit data-loss warnings and an approval gate before anything destructive runs. Use whenever a ticket asks to "wipe", "reset", "clean up", or "remove company data from" a device.
category: M365 Administration
tools: [search_tickets, search_contacts, search_knowledge_base, search_itglue, search_hudu, add_ticket_note, update_ticket, send_approval, log_time_entry, web_search]
---

# Device Wipe Workflows

"Wipe it" arrives as one phrase but names five different actions with five
different blast radii — from removing only company data to destroying
everything on the disk. This skill maps the intent to the least destructive
action that achieves it, states the data loss in plain language, and gates
execution behind recorded approval.

## When to use

- "Wipe <user>'s laptop — they're leaving" / offboarding device cleanup.
- "This machine is a mess, reset it" / handing a device to a new user.
- "Remove company data from <user>'s personal device" (BYOD).
- A failed deployment or corrupted profile where reset is proposed
  (cross-check autopilot-deployment before defaulting to a wipe).
- Lost or stolen device → the time-critical path lives in mobile-device-mdm;
  use this skill for the action-selection detail once that runbook engages.

## Steps

1. **Establish intent, ownership, and data reality before naming an
   action:** What outcome does the requester actually want? Corporate or
   personal (BYOD) device? Does the device hold data that exists nowhere
   else (local files, un-synced folders — ask explicitly; "everything's in
   OneDrive" is a claim to verify, not an assumption)? Who is the authorized
   requester — for offboarding wipes, that is the client authority, never
   the departing user's word alone.
2. **Map to the decision tree, least destructive first:**
   - **Retire** — removes company data, apps, and management; personal data
     untouched. The default for BYOD offboarding (this is the selective
     wipe app-protection-policies promises) and for devices leaving
     management but staying with their owner.
   - **Autopilot Reset / Wipe with reprovision intent** — rebuilds the OS
     and re-enrolls for the same tenant; user data on the device is
     destroyed. For handing a corporate device to a new user or recovering
     from a broken deployment.
   - **Fresh Start** — reinstalls Windows removing manufacturer bloat; the
     keep-user-data option exists but removes apps. Narrow use: cleaning up
     OEM images.
   - **Wipe (factory reset)** — everything on the device is destroyed. For
     device disposal, return-to-lessor, or confirmed-loss scenarios.
   - **Delete (record only)** — removes the Intune/Entra record and touches
     the device not at all. Never the answer to "wipe it"; pairs with
     stale-device-cleanup rules (BitLocker keys die with the object —
     preserve first).
   Verify current action behavior per platform against Microsoft docs
   (web_search) — remote-action semantics differ by platform and change.
3. **Pre-wipe salvage.** For any OS-destroying action: confirm data backup
   state (sync health, local-only data recovered), retrieve and preserve the
   BitLocker recovery key BEFORE the action (see bitlocker-key-retrieval —
   post-wipe recovery needs it if anything goes sideways), and note license
   or app deactivations that need doing while the OS still boots.
4. **Approval gate — always, with consequences stated plainly.**
   send_approval to the authorized client contact naming: the device, the
   exact action, what is destroyed ("all data on the device, unrecoverable"
   vs "company data only; personal files untouched"), what survives, and
   the point of no return. For BYOD retires, the device owner is informed
   before execution — company data vanishing from a personal phone without
   warning reads as an attack. No destructive action executes without the
   recorded approval; when in doubt about authorization, do nothing and
   escalate.
5. **Execute and verify.** Tech triggers the action; note that an offline
   device executes the wipe whenever it next checks in — a pending wipe is
   a live grenade, so record it prominently and reconcile if the request is
   later cancelled (cancel the pending action explicitly; do not assume it
   expired). Verify completion in the console (action status) and, where
   relevant, that re-enrollment/reprovisioning succeeded.
6. **Document.** Plain-text note: device, action chosen and why the less
   destructive options were insufficient, data-salvage steps done, approver
   and approval reference, execution time, verification, and follow-ups
   (record deletion, asset disposal, license reclaim). Log time
   (log_time_entry).

## Guardrails

- No wipe, retire, fresh start, or reset without recorded approval that
  names the data-loss consequence — urgency does not waive the gate; the
  lost/stolen path in mobile-device-mdm carries its own approval discipline.
- Always propose the least destructive action that meets the intent, and
  record why anything stronger was chosen.
- BitLocker key preservation precedes every OS-destroying action on an
  encrypted device.
- "The user said it's all backed up" is verified (sync status, spot check),
  not trusted — the wipe is unrecoverable and the claim is free.
- Pending wipes on offline devices are tracked to completion or explicit
  cancellation; an orphaned pending wipe firing weeks later is a
  self-inflicted incident.
- Never use record deletion as a substitute for a device action, and never
  wipe to troubleshoot enrollment (see intune-enrollment-troubleshooting).
