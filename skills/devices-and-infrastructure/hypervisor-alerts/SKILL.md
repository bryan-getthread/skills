---
name: Hypervisor Alert Triage
description: Triage Hyper-V/VMware host alerts — datastore/volume capacity, snapshot and checkpoint sprawl, host CPU/memory pressure — and decide whether the problem is host-level or VM-level before anyone touches anything. Use when a virtualization host alerts, guests slow down together, or a datastore fills.
category: Devices & Infrastructure
tools: [search_ninjaone_devices, get_ninjaone_device, list_ninjaone_alerts, get_ninjaone_device_activities, get_ninjaone_device_link, reset_ninjaone_alert, search_itglue, search_hudu, search_tickets, add_ticket_note, create_ticket, update_ticket]
---

# Hypervisor Alert Triage

Host alerts punch above their weight — one sick host is every guest's bad day. This skill reads the alert, separates host-level causes from VM-level ones, and hands the tech a specific, ordered remediation instead of "the host is red".

## When to use

- A Hyper-V or VMware/ESXi host raises a capacity, resource, or health alert.
- Several VMs on the same client degrade at once (the classic shared-host signature).
- A datastore/CSV/volume is filling and someone must decide what to delete or move.
- Recurring host alerts that keep getting reset without a fix.

## Steps

1. Confirm you are looking at a host: `get_ninjaone_device` and documentation (`search_itglue` / `search_hudu`) — hostname prefixes (HV/ESX/VMH) are hints, not facts. From documentation, map the host's guests and the storage backing it (local, SAN/NAS, cluster shared volume); the guest map determines the blast radius of everything that follows.
2. Read the alert family from `list_ninjaone_alerts` and recent `get_ninjaone_device_activities`, then run the host-vs-VM split — the core judgment: all/most guests affected, or host-level metrics (CPU ready/queueing, memory pressure, datastore latency) elevated → host-level; one guest misbehaving while the host is comfortable → VM-level, route as a normal server ticket for that guest (`device-health-check` / `server-diagnostics`) and say the host is exonerated with evidence.
3. Datastore/volume capacity alerts — find what is actually consuming: the usual suspects in order: snapshot/checkpoint sprawl (long-running snapshots grow unbounded and are the top emergency cause), orphaned VM disks from deleted VMs, thin-provisioned disks growing into overcommitted space, and ISO/backup files parked on the datastore. A datastore at genuine risk of filling is an emergency — guests pause or crash when it hits zero; escalate priority accordingly (`update_ticket`).
4. Snapshot sprawl specifically: inventory snapshots/checkpoints by age and size (via the hypervisor console — a hands-on step to hand off with `get_ninjaone_device_link`). Deleting/consolidating a large old snapshot generates heavy I/O and can take hours — it belongs in a window, not a business-hours panic click, unless the datastore is critically full and the risk trade goes the other way; spell out that trade in the recommendation. Check `search_tickets` for why the snapshot exists — a backup product's stuck snapshot points at the backup chain (`backup-failure-triage`), not manual cleanup.
5. Host resource pressure: check consolidation history (`get_ninjaone_device_activities`, ticket history) for recently added or resized guests; identify the noisy neighbor where guest metrics allow. Remediation ladder: rebalance/migrate guests, right-size overallocated VMs, then hardware. Host reboots are last-resort choreography — every guest must be gracefully handled first; never propose a host reboot with the casualness of a workstation reboot.
6. Output: host-vs-VM verdict with evidence, ranked cause hypothesis, remediation steps in order with window requirements, and the deep link. Reset the alert (`reset_ninjaone_alert`) only after a real action or verified condition change, with a plain-text note (`add_ticket_note`) saying what was found and done. Chronic alerts get a root-cause ticket (`create_ticket`), not a weekly reset.

## Guardrails

- Never recommend deleting snapshots, disks, or files without stating what they are and confirming the backup product is not mid-chain on them — deleting a backup's working snapshot corrupts the chain.
- Host reboots and storage migrations are change-window work with the full guest map in hand; the RMM MCP cannot orchestrate them — hands-on handoff only.
- Do not reset a host alert to clear the board; resets without remediation are recorded as such ("reset, unresolved, recurrence expected").
- Capacity math from a single reading is a snapshot, not a trend — for "when does it fill", route to `storage-capacity-planning`.
- If the host is not RMM-managed (common for ESXi), say monitoring visibility is limited to what documentation and guest symptoms show, and route to the hypervisor console.
- Notes are plain text — no markdown, no emojis.

## Unattended (Flows) variant

- Your entire reply is posted verbatim as the ticket note — no narration, no questions, plain text only.
- Do the host-vs-VM split and cause ranking from tool evidence only; if the evidence does not distinguish, state both possibilities and stop.
- Never reset alerts, never change ticket priority upward of the board's rules, never recommend deletions in unattended mode — findings and a recommended next assignment only.
- If the device cannot be confirmed as a hypervisor host, output the single line stating that and do nothing else.

## See also

- `storage-capacity-planning` — trend-based "when does it fill" forecasting.
- `server-patch-windows` — host patching choreography.
- `backup-failure-triage` — when snapshots trace back to the backup chain.
