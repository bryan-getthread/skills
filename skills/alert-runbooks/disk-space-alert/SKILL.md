---
name: Disk Space Alert
description: Triage a low-disk-space alert regardless of which monitor raised it — separate threshold noise from real pressure, read the growth rate from the alert history, and route with ranked consumer hypotheses. Use when a disk/volume-space alert lands on the board and needs a verdict, not yet a cleanup.
category: Alert Runbooks
tools: [search_tickets, search_ninjaone_devices, get_ninjaone_device, list_ninjaone_alerts, get_ninjaone_device_activities, add_ticket_note, update_ticket]
---

# Disk Space Alert

The alert-layer triage for disk-space alerts: decide whether the alert is a threshold artifact, a slow creep, or an act-now emergency, and route it with a growth-rate estimate and consumer hypotheses. The hands-on cleanup itself is the Disk Space Remediation skill — this skill decides whether and how urgently to invoke it.

## When to use

- A "disk space below X%" alert ticket lands on the intake or alerts board.
- The same volume keeps alerting and someone asks "is this real or just the threshold?"
- A Flow sweeps disk alerts and needs a deterministic close/route/escalate verdict (see Unattended variant).

## Steps

1. Parse the alert anatomy: device, volume/drive letter, threshold that fired (percent vs absolute GB), current reading, and timestamp. Percent thresholds on very large volumes mislead — 5% of a 4 TB volume is 200 GB of headroom; convert to absolute free space before judging severity.
2. Dedupe/recurrence check via `search_tickets`: same device + disk alert, past 30 days. If an open ticket for the same volume exists, this is a duplicate — note and merge/route to it, don't work it twice. Flag if the search may have hit a result cap.
3. Verify current state via `get_ninjaone_device` (resolve with `search_ninjaone_devices` if needed): read the live volume numbers. The alert is a snapshot; the disk may have recovered (temp files flushed, backup staging cleared) or worsened since it fired.
4. Read the growth rate from repeated alerts: `list_ninjaone_alerts` history for this device/volume. A volume that crossed 85% → 90% → 95% across days is filling on a trend — compute a rough days-to-full from the interval between alerts and state it. A single crossing with recovery is churn around the threshold.
5. Correlate with `get_ninjaone_device_activities`: a patch run, backup job, or application install just before the alert explains a spike; nothing preceding it suggests organic growth (logs, profiles, data).
6. Classify:
   - Self-healed: current free space is comfortably back above threshold AND no repeat pattern → close as recovered with a note.
   - Needs-tech: real pressure (under threshold now, or trend says days-to-full is short) → route with severity (system volume under 5% / 5 GB = act-now) and ranked consumer hypotheses by device role (workstation: profiles/caches/updates; server: logs/backup staging/databases/shadow copies). Hand the actual cleanup to the Disk Space Remediation skill.
   - Needs-client: the volume is a client-managed data share filling with business data — capacity conversation, not cleanup; route to account owner.
   - Noise: threshold misconfigured for the volume size (percent alert on multi-TB data volume with hundreds of GB free) → recommend a threshold change, do not just close and let it re-fire.
7. Output/post: verdict, absolute free space now, growth-rate estimate (or "single event, no trend"), hypotheses, and recommended route — plain-text note via `add_ticket_note`.

## Guardrails

- Consumer hypotheses are inferred, never observed — the RMM does not expose per-folder usage. Label them hypotheses.
- Never close a repeat-offender volume as recovered; three alerts on the same volume in 30 days is a capacity/root-cause problem regardless of current reading.
- Do not recommend deletions here — that is the remediation skill's job, with its safety ordering.
- If NinjaOne is not enabled, degrade to alert text + ticket history and say the live disk view is unavailable.
- Plain-text notes only.

## Unattended (Flows) variant

- Your entire reply is posted verbatim as the ticket note — plain text, no narration, no markdown.
- Deterministic gates: close-as-recovered ONLY when current live reading is above threshold with margin (≥5 percentage points or ≥10 GB above) AND fewer than 3 alerts on this volume in 30 days AND no open sibling ticket. Otherwise route: act-now severity → escalate to the on-call/alerts queue with the days-to-full estimate; anything else → route to the tech queue with the classification note.
- If the device is unreachable or the live reading cannot be pulled, do not close — route to a human with "current state unverifiable."
