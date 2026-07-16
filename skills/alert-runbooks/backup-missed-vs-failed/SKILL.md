---
name: Backup Missed vs Failed Alert
description: Distinguish a backup job that never ran (missed) from one that ran and errored (failed) — two different diagnoses and routes — and always state the client's actual exposure via last-known-good. Use when a backup alert is ambiguous about whether the job executed at all.
category: Alert Runbooks
tools: [search_tickets, get_ninjaone_device, get_ninjaone_device_activities, list_ninjaone_alerts, liongard_metric, liongard_launchpoint, search_itglue, add_ticket_note, update_ticket]
---

# Backup Missed vs Failed Alert

"Backup did not complete" hides two different problems: a job that never started (scheduling/availability — the device was off, the service was down, the window was skipped) and a job that started and died (an error with a taxonomy). This skill makes the missed-vs-failed call first, because everything downstream depends on it, and always ends with an exposure statement. Deep failure-mode classification lives in Backup Failure Triage.

## When to use

- A backup alert reads "missed", "did not run", "overdue", or is ambiguous between missed and failed.
- "Did last night's backup for <client> actually run?"
- A Flow sweeps morning backup alerts and needs deterministic routing (see Unattended variant).

## Steps

1. Parse the alert anatomy: device/job name, scheduled window, whether the alert carries an error code (errors imply the job ran) or only an overdue/missed condition (implies it never started), and the product raising it.
2. Make the missed-vs-failed call explicit:
   - FAILED = the job executed and returned an error → there is an error to classify; hand the taxonomy work to Backup Failure Triage.
   - MISSED = no job execution in the window → the diagnosis is why it never started, not what error it hit. Do not treat an overdue alert as a failure.
3. Dedupe/recurrence via `search_tickets`: same device/job, past 30 days. Chronic missed jobs (device off every night, laptop never on the schedule) are a schedule-design problem; chronic failures are a product problem. State which pattern this is.
4. Verify why a missed job missed: `get_ninjaone_device` — was the device online during the window? `get_ninjaone_device_activities` — was it asleep, rebooting, or mid-patch? `list_ninjaone_alerts` — was the backup service stopped? A laptop that was in a bag at 2 a.m. is a schedule problem (wake settings, run-on-connect), not an incident.
5. Where the backup platform has a Liongard inspector, read job history via `liongard_metric` (verify the inspector via `liongard_launchpoint` and state dataprint age) to confirm the last successful run and whether a later run has since succeeded. Cross-check documentation (`search_itglue`) for the intended schedule and retention design.
6. Classify:
   - Self-healed: a subsequent run of the same job completed successfully → close, citing the successful run.
   - Needs-tech: failed jobs (route into Backup Failure Triage), backup service down, or a missed window on a server that should always be available.
   - Needs-client: device-availability misses on client-controlled machines (laptops powered off, desktops unplugged) → client conversation about availability or schedule change.
   - Noise: a one-time miss during a documented maintenance window with a clean run after it.
7. Exposure statement — mandatory in every output regardless of class: "Last known good backup for <device/job>: <date/time>. Data changed since then is unprotected." If last-known-good cannot be established, say exactly that — it makes the ticket more urgent, not less. Post via `add_ticket_note` (plain text).

## Guardrails

- The exposure statement is non-negotiable: no output from this skill omits last-known-good or an explicit "unknown".
- Never say data is safe or a restore will work — report job evidence only; restore verification is a human task.
- Never close a missed-backup alert because the device "was probably off" — verify device state in the window or route to a human.
- A recurring miss is a schedule/design problem; do not close the third one as a one-off.
- If neither the RMM nor a Liongard inspector can see the backup platform, say the view is partial and name what to check in the backup console.
- Plain-text notes only; do not clear backup alerts — they are the evidence trail.

## Unattended (Flows) variant

- Your entire reply is posted verbatim as the ticket note — plain text, no narration — and it MUST contain the exposure statement.
- Deterministic gates: close ONLY when a subsequent successful run of the same job is evidenced (fresh inspector data or explicit success event) AND recurrence is under 3 in 30 days. Failed (error present) → route to backup-triage queue. Missed with device offline in window → route as availability/schedule issue. Missed with device online → escalate (service or product fault).
- Last-known-good unknown, stale dataprint, or capped search → route to a human; never close.
