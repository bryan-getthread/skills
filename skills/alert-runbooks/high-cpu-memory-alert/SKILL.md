---
name: High CPU/Memory Alert
description: Triage a CPU or memory threshold alert — separate a transient spike from sustained pressure using alert history, offer top-consumer hypotheses by device role, and route down different paths for servers vs workstations. Use when a performance-threshold alert lands from any monitor.
category: Alert Runbooks
tools: [search_tickets, search_ninjaone_devices, get_ninjaone_device, list_ninjaone_alerts, get_ninjaone_device_activities, get_ninjaone_device_link, add_ticket_note, update_ticket]
---

# High CPU/Memory Alert

A CPU pegged for 90 seconds during an AV scan and a server sitting at 95% for six hours produce the same alert. This skill separates spike from sustained, names likely consumers as hypotheses, and routes servers and workstations down their different paths.

## When to use

- A "CPU above X%" or "memory above X%" alert opens a ticket.
- "<device> keeps alerting on performance — real or noise?"
- A Flow sweeps performance alerts and needs a deterministic verdict (see Unattended variant).

## Steps

1. Parse the alert anatomy: device, metric (CPU vs memory — different playbooks), threshold, observed value, and the sample duration if the monitor states one. An alert on a 5-second sample means almost nothing; one on a 15-minute average means a lot.
2. Dedupe/recurrence via `search_tickets` and `list_ninjaone_alerts`: same device + same metric, 30 days. The alert history IS the spike-vs-sustained instrument: one alert with recovery = spike; alerts recurring daily at the same hour = scheduled-workload pattern (backup, scan, report job); continuous or escalating alerts = sustained pressure.
3. Verify current state via `get_ninjaone_device` (resolve with `search_ninjaone_devices` first if needed): current utilization, uptime, and device class. Long uptime plus creeping memory alerts on the same device suggests a leak pattern — for servers, hand that specific case to the Low Memory Server Alert skill.
4. Correlate `get_ninjaone_device_activities` with the alert timestamps: patch installs, AV scans, backup jobs, and update downloads are the classic benign spike causes; a new application install just before pressure began is a prime suspect.
5. Offer top-consumer hypotheses by role — labelled as hypotheses, since the RMM view does not show live per-process data: workstations — browser tab sprawl, AV scan, OneDrive/sync clients, video calls, runaway update process; servers — database engine growth, IIS/app pool leak, backup/dedup jobs, terminal-server user load, runaway logging.
6. Split the path by device class:
   - Workstation: sustained pressure usually means a reboot cures it or the hardware is undersized. Route needs-tech with hypotheses; if the user reports slowness, that ticket and this alert should merge.
   - Server: never suggest a casual reboot; sustained pressure is a capacity or workload-fault investigation (see Server Diagnostics) and any restart goes through a maintenance window. Multiple users are affected — weight urgency accordingly.
7. Classify: self-healed (spike recovered, no recurrence pattern) → close with the recovery evidence; needs-tech (sustained, or recurring outside a benign scheduled-job correlation) → route with hypotheses and `get_ninjaone_device_link`; needs-client (client-owned workload, e.g. their LOB app consuming a server they manage) → account owner; noise (threshold too tight for the device's normal duty cycle — alerts every backup window) → recommend threshold/schedule tuning, do not just close.
8. Output/post: metric, spike-vs-sustained verdict with the alert-history evidence, hypotheses, path, and route — plain-text note via `add_ticket_note`.

## Guardrails

- Never reboot or kill anything from this skill; it is a triage layer. Restart decisions live in the reboot/maintenance workflows with their confirmations.
- Consumer lists are hypotheses, not observations — say so; the tech confirms with live process data.
- A daily-recurring alert correlated to a scheduled job is tuning noise, but confirm the job exists in activities before calling it that.
- Recurring sustained pressure is a capacity problem — do not present the third "it recovered" as resolution.
- If NinjaOne is not enabled, degrade to alert text + ticket history and say the live view is unavailable.
- Plain-text notes only.

## Unattended (Flows) variant

- Your entire reply is posted verbatim as the ticket note — plain text, no narration.
- Deterministic gates: close ONLY when current utilization is back under threshold AND this is the first such alert on the device in 30 days. Recurrence 2+ correlated to the same scheduled window → route as threshold-tuning with the pattern stated. Sustained now (still above threshold at triage) on a server → escalate; on a workstation → route to tech queue. Never close a server alert on recovery alone if recurrence ≥2.
- Unverifiable current state → route to a human; never close.
