---
name: Backup Failure Triage
description: Classify a backup-failure alert into its failure mode using the alert text plus device state, check for recurrence, and decide fix-here vs escalate-to-vendor. Use when a backup job failed, a backup alert lands on the board, or someone asks "did backups run for <client>".
category: Devices & Infrastructure
tools: [list_ninjaone_alerts, get_ninjaone_device, get_ninjaone_device_activities, search_tickets, search_itglue, add_ticket_note]
---

# Backup Failure Triage

Turns a raw backup-failure alert into a classified failure mode with a recurrence verdict and a clear "handle here" or "escalate to vendor" recommendation.

## When to use

- A backup-failure alert or backup-monitoring ticket needs triage.
- "Backups failed again on <device> — what's going on?"
- "Have backups for <client> been failing repeatedly?"

## Steps

1. Read the alert text carefully — backup products embed the failure reason. Pull the source device with `get_ninjaone_device` and its recent `get_ninjaone_device_activities` in parallel.
2. Classify the failure mode from alert text + device state:
   - Device offline / unreachable at job time → scheduling/availability problem, not a backup problem.
   - Destination full or quota exceeded → capacity problem; check disk/storage numbers on the device.
   - VSS / snapshot / writer errors → OS-level; often follows a pending reboot or a recent patch (check activities).
   - Credential / authentication failures → recently rotated passwords or service-account changes.
   - Network/timeout to the backup target → path or bandwidth issue; check whether other devices at the site also failed.
   - Agent/version errors → recent agent update in the activities log.
3. Recurrence check via `search_tickets`: same device, same failure class, past 30–90 days. One failure with a successful run after it is noise; three failures of the same class is a problem ticket. State the recurrence verdict explicitly, and flag if the search may have hit a result cap.
4. Check documentation (`search_itglue`) for the client's backup product, retention design, and any documented known issues or vendor support contacts.
5. Decide the path:
   - Handle here: offline-at-job-time, pending-reboot VSS, obvious destination-full — these have local remediations a tech can do.
   - Escalate to vendor when: repeated failures of the same class after local remediation, corruption/integrity errors, failures across many clients simultaneously (possible product-side incident), or anything the vendor's own documentation marks as support-required. Name the vendor generically from the docs — do not guess the product.
6. Output: failure classification, evidence, recurrence verdict, recommended action, and — critically — the last known good backup date, because that is the client's actual exposure. Offer to post a plain-text note via `add_ticket_note`.

## Guardrails

- Never state that data is safe or that a restore will work — only report the last successful job on record. Restore verification is a human task.
- Do not clear or reset backup alerts as part of triage; the alert is the evidence trail.
- A recurring failure must never be closed as a one-off — recommend a problem ticket instead.
- If the backup product is not visible through RMM alerts (many run their own consoles), say the view is partial and name what the tech should check in the backup console.
- Plain-text notes only.

## Unattended (Flows) variant

- Follows the Unattended Output Discipline contract: the entire reply is the plain-text triage note posted verbatim — failure classification, evidence, recurrence verdict, last known good backup date, and the handle-here vs escalate-to-vendor recommendation. No narration.
- Deterministic inputs from the flow: the alert or ticket id carrying the alert text. If the device cannot be resolved unambiguously, classify from alert text alone and state `DEVICE UNRESOLVED - alert-text classification only` inside the note; if the alert text yields no classification at all → output nothing.
- Recurrence counts from capped searches are stated as "at least N" inside the note.
- Permitted writes: `add_ticket_note` only. Never reset or clear alerts (the alert is the evidence trail), never close tickets, never touch the device.
- Never state data is safe or that a restore will work — last successful job on record is the strongest claim, in any mode.
