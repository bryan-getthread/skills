---
name: Synology NAS Alerts
description: A Synology NAS alert needs working — degraded RAID/storage pool, disk health warnings, volume nearly full, or DSM update decisions. Treat a degraded array as one failure from data loss and keep disk-replacement discipline.
category: Vendor Runbooks
tools: [search_tickets, search_itglue, add_ticket_note, update_ticket]
---

# Synology NAS Alerts

Vendor runbook for Synology NAS devices (DSM alerts arriving by email or RMM forwarding). The organizing fact: a NAS alert is usually a storage-redundancy event, and redundancy math is unforgiving — a degraded array is not "still working," it is "working with zero margin." Verify DSM menu paths and behavior against Synology's current documentation; DSM versions differ.

## When to use

- A "storage pool/volume degraded" or disk-failure alert lands for a client's Synology
- SMART / disk-health warnings arrive (bad sectors increasing, reallocated sectors)
- A volume-nearly-full alert or a DSM-update decision needs handling

## Steps

1. Parse the alert: device name/model, the affected storage pool/volume, the alert class (degraded, disk failure, SMART warning, volume capacity, DSM update available, service/security notifications), and which drive slot if named. Pull the client's documentation (search_itglue) for the unit's RAID layout, drive inventory, and what workloads live on it — a NAS holding the client's backups failing is a different emergency than a media share.
2. Degraded pool / failed disk → the priority path:
   - State the redundancy position plainly: with one disk down, an SHR/RAID-5-class array survives zero further failures; RAID-6/SHR-2 survives one. That sentence goes in the note and drives the clock.
   - Verify backups of the NAS's data FIRST — before any physical work: last successful backup of the affected volumes (the NAS's own backup jobs or the client's backup product). Rebuilds stress the remaining disks; the highest-risk window for a second failure is the rebuild itself. No current backup → escalate the backup gap before touching drives.
   - Disk-replacement discipline for the technician: identify the failed drive by slot number AND serial from DSM Storage Manager — never by "the one with the red light" alone; confirm the replacement drive meets size/type requirements (and the client's compatibility expectations); replace one drive at a time; monitor the rebuild to completion and record its finish. Pulling the wrong healthy disk from a degraded array is the classic total-loss move — the slot/serial double-check exists to prevent exactly that.
3. SMART/health warnings on a still-healthy array → pre-failure signal: check the drive's age and error trend (increasing reallocated/pending sectors = replace proactively, on schedule, with the same discipline), and check search_tickets for prior warnings on the same unit — multiple aging drives from one purchase batch tend to fail together; recommend staggered proactive replacement.
4. Volume nearly full → find the consumer before adding space: snapshots retention, recycle bins, backup targets growing unbounded, or genuine data growth. Cleanup of snapshots/recycle data follows the client's retention intent (search_itglue), never ad-hoc deletion. Persistent growth → capacity planning conversation, routed to account management.
5. DSM update discipline: security updates matter (NAS devices are actively targeted when exposed), but updates reboot the unit and occasionally change behavior — schedule within the client's maintenance window, verify current backups exist first, and never trigger during business hours as ticket housekeeping. If the unit is internet-exposed (QuickConnect/port-forwards) and behind on security updates, flag that as a security finding, not just hygiene.
6. Document: alert class, redundancy position, backup-verification result, actions directed vs completed, and rebuild status if applicable. Physical and DSM actions are technician steps the agent directs and records.

## Guardrails

- A degraded array ticket never waits in the normal queue — zero-margin storage gets an urgent clock, and the note says why.
- Backup verification before rebuild, always — the rebuild is the risk window, and "the NAS is the backup" is a finding in itself (flag single-copy data loudly).
- Never direct a drive pull without slot + serial confirmation from Storage Manager; one wrong pull on a degraded array is unrecoverable.
- Never state data is safe during/after a rebuild — report array status and last-backup facts only.
- No ad-hoc deletion of snapshots or recycle bins to clear space — retention follows the client's documented intent.
- Repeated disk failures or an aging same-batch fleet → problem ticket and proactive-replacement recommendation, never serial one-off closes.
- Degradation: without documentation the RAID layout may be unknown — say so; the technician confirms in DSM before anything physical.
