---
name: RAID Degradation Alert
description: Triage a RAID degraded/failed-member alert with zero-margin urgency — a degraded array is one failure from data loss — and enforce the verify-backups-BEFORE-rebuild rule. Use for any degraded-array, failed-disk-in-array, or rebuild alert from a server, NAS, or storage controller.
category: Alert Runbooks
tools: [search_tickets, get_ninjaone_device, get_ninjaone_device_activities, list_ninjaone_alerts, liongard_metric, liongard_launchpoint, search_itglue, add_ticket_note, update_ticket]
---

# RAID Degradation Alert

A degraded array still serves data, which is exactly why these alerts get parked — and why they are dangerous. Redundancy is already spent: the next failure, including one triggered by rebuild stress, is data loss. This skill routes with that zero-margin framing and hard-codes the one rule that saves clients: verify backups before anyone touches the array.

## When to use

- "Array degraded / disk failed in array / RAID rebuild started" alerts from servers, NAS units, or storage controllers.
- "How urgent is this degraded-array ticket really?"
- A Flow encounters a RAID alert and must route it correctly (see Unattended variant).

## Steps

1. Parse the alert anatomy: device, array/volume identifier, RAID level if stated, which member failed, and whether a rebuild is already running. RAID level sets the margin: RAID 5 degraded = zero redundancy remaining; RAID 6 or a mirrored pair with one loss may retain one margin — state which case this is, or "level unknown" if the alert does not say.
2. Dedupe/recurrence via `search_tickets`: same device + array alerts, 90 days. A second member failure or repeated degradation on the same chassis is an aging-batch signal — drives from one purchase batch fail together; flag that the remaining members are suspect.
3. Verify current state: `get_ninjaone_device` for device health, `list_ninjaone_alerts` for accompanying SMART or predictive-failure alerts on other members (see SMART Disk Warning — a SMART warning on a second member during degradation is an emergency). Where a Liongard inspector covers the NAS/storage platform, read array state and rebuild progress via `liongard_metric` (verify inspector freshness via `liongard_launchpoint`, state dataprint age). For NAS-specific handling, cross-reference the Synology NAS Alerts skill.
4. THE RULE — backups before rebuild: before recommending replacement or rebuild, establish the last known good backup of the data on this array (`search_itglue` for the backup design; backup job evidence per Backup Missed vs Failed). Rebuild stress is a classic second-failure trigger. If backups are current → proceed to replacement urgency. If backups are stale or unknown → the FIRST action is a fresh backup/copy of critical data, and the note must say so in exactly that order.
5. Classify — with a deliberately narrow self-heal lane:
   - Self-healed: rebuild completed and the array reports optimal/healthy in current (fresh) data → close with the evidence and a recommendation to watch the replaced member.
   - Needs-tech (the default): degraded, rebuilding, or ambiguous → act-now route: hardware replacement is physical work with a procurement step (drive model/size from documentation), and hot-spare presence changes the timeline. Never noise, never needs-client-only.
6. Output/post: array state, RAID level and remaining margin, backup status (the exposure statement), recurrence/batch signal, and the ordered action list (verify/refresh backup → replace member → monitor rebuild) — plain-text note via `add_ticket_note`.

## Guardrails

- Exposure statement is mandatory: last known good backup for the data on this array, or an explicit "unknown — treat as unprotected."
- Never recommend rebuild or member replacement before the backup check; never present a running rebuild as resolution — rebuilds fail.
- A degraded array is never noise and is never closed on assumption; only a verified optimal/healthy state closes it.
- Do not clear the RAID alert — it is the evidence trail while the array is at reduced redundancy.
- RMM visibility into RAID controllers is often shallow; if array state is not directly readable, say the view is partial and name the controller/NAS console the tech must check.
- Plain-text notes only.

## Unattended (Flows) variant

- Your entire reply is posted verbatim as the ticket note — plain text, no narration — and must contain the backup-status exposure statement.
- Deterministic gates: the ONLY close is verified array-optimal in fresh data after a completed rebuild, with no other member alerts. Everything else → escalate to the urgent/hardware queue, with priority raised further when (a) backups are stale/unknown, (b) a second member shows SMART/predictive warnings, or (c) RAID level leaves zero remaining redundancy.
- Any ambiguity about array state → escalate; never close, never downgrade.
