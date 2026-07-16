---
name: Disk Space Remediation
description: Work a disk-pressure alert or "drive is full" ticket — establish what is likely consuming space from available RMM signals, give the tech a safe-cleanup sequence, and hand off via device deep link. Use when a low-disk alert fires or a user reports a full drive.
category: Devices & Infrastructure
tools: [get_ninjaone_device, search_ninjaone_devices, list_ninjaone_alerts, get_ninjaone_device_activities, get_ninjaone_device_link, search_knowledge_base, add_ticket_note]
---

# Disk Space Remediation

Turns a low-disk alert into an actionable brief for the tech: severity, likely consumers, a safe-cleanup order of operations, and the deep link to go do it — because the agent cannot run cleanup scripts itself.

## When to use

- A disk-space alert lands on the board.
- "<user>'s C: drive is full" / "server <device> is at 98% on D:."
- A health check or fleet sweep flagged disk pressure and someone picks up the ticket.

## Steps

1. Resolve the device (organization first, rank candidates by org match then last-contact) and pull `get_ninjaone_device` for current volume numbers. Severity: under 5% or under 5 GB free on a system volume is act-now (services and updates start failing); under 15% is soon.
2. Read the evidence available without touching the machine: which volume is full (system vs data changes the playbook), `list_ninjaone_alerts` for how long pressure has been building (slow creep vs sudden spike), `get_ninjaone_device_activities` for correlating events — a failed patch run (leftover update files), a backup job writing locally, a recent application install.
3. Check whether another tech is already on the device before proposing anything.
4. Infer likely consumers by device role: workstations — user profiles, downloads, OneDrive/cache bloat, shadow copies, Windows Update leftovers; servers — log growth (IIS/SQL/app logs), backup staging, WSUS/update caches, database files, shadow copies; sudden spikes — a runaway log or dump file. Present these as hypotheses ranked by the evidence, not findings.
5. Give the tech the safe-cleanup sequence, in order of decreasing safety: (a) empty recycle bins and temp/update caches via Disk Cleanup/Storage Sense; (b) clear stale user profiles per policy; (c) prune/archive application logs after confirming nothing needs them; (d) shrink shadow-copy allocation only with understanding of restore-point impact; (e) anything touching databases, backup chains, or application data is change-controlled — not cleanup. Explicitly: never delete files you cannot identify, and moving data beats deleting it.
6. Check `search_knowledge_base` for a client- or device-specific cleanup SOP and link it if one exists.
7. Hand off with `get_ninjaone_device_link` — remediation is hands-on tech work; this integration cannot execute scripts or deploy cleanup tools. Offer to post the brief as a plain-text note via `add_ticket_note`.
8. Close the loop: after the tech confirms cleanup, re-check `get_ninjaone_device` and report before/after free space. If pressure returns within days, recommend a root-cause ticket (growth source, quota, or storage expansion) rather than repeat cleanup.

## Guardrails

- NO script execution, NO automated deletion — this integration cannot and must not attempt it. Every destructive step is performed by the tech, by hand, with the sequence above as guidance.
- Consumers are inferred, not observed — the RMM does not expose per-folder usage. Say so; the tech verifies on the machine.
- On servers, anything beyond temp/log hygiene needs a maintenance window and user-impact confirmation first.
- Recurring pressure is a capacity problem; do not present the third cleanup as a fix.
- If NinjaOne is not enabled, degrade to generic guidance plus ticket history, and say the live disk view is unavailable.
- Plain-text notes only.

## Unattended (Flows) variant

- Follows the Unattended Output Discipline contract: the entire reply is the plain-text remediation brief posted verbatim — severity (volume, free space, act-now vs soon), likely consumers explicitly labeled as hypotheses, the safe-cleanup sequence, and the device deep link from `get_ninjaone_device_link`. No narration.
- Deterministic inputs from the flow: the device id from the triggering disk alert. Device unresolvable or volume data unreadable → output nothing.
- Another tech's recent remote-session or manual activity on the device → the brief leads with `TECH ALREADY ENGAGED - coordinate before acting`.
- Repeat pressure (prior disk alerts within the flow's lookback) → the brief recommends a root-cause/capacity ticket instead of presenting another cleanup as the fix.
- Permitted writes: `add_ticket_note` only. No script execution, no deletion, no device actions of any kind — remediation is hands-on tech work in every mode, and doubly so unattended.
