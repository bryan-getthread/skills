---
name: Acronis Cyber Protect
description: An Acronis Cyber Protect alert arrived — first decide whether it is a backup failure or an Active Protection (anti-ransomware/security) detection, then run the matching discipline; the two must never be triaged the same way.
category: Vendor Runbooks
tools: [search_tickets, search_itglue, get_ninjaone_device, get_ninjaone_device_activities, get_ninjaone_device_link, add_ticket_note, update_ticket]
---

# Acronis Cyber Protect

Vendor runbook for Acronis Cyber Protect, the combined backup + security product. Its alerts arrive in one stream but belong to two disciplines: backup failures (→ backup-failure-triage / the Veeam-style taxonomy) and Active Protection / antimalware detections (→ edr-detection-runbook). The classification step is the vendor-specific skill — a ransomware detection mis-triaged as "a backup warning" is the worst possible miss. Verify feature names against Acronis's current documentation.

## When to use

- An Acronis alert lands and it isn't obvious whether it's backup or security
- An Active Protection / ransomware-detection alert fires on a protected machine
- An Acronis backup job failure needs classification and an exposure statement

## Steps

1. Classify the alert stream first — backup event or security event:
   - Backup: job failed/warning, storage/quota, agent offline at job time, validation failure.
   - Security: Active Protection detection (suspicious file-modification patterns — the ransomware heuristic), antimalware detection, self-defense/tamper events against the Acronis agent itself.
   - Ambiguous → treat as security until classified; the cost asymmetry demands it.
2. Security path — run edr-detection-runbook with the Acronis specifics:
   - Active Protection ransomware heuristics: note what the product did — blocked the process and/or reverted affected files from its cache. Reverted files are containment of symptoms; the process, its origin, and persistence still need working. Device context via get_ninjaone_device / activities; user corroboration via a verified channel (backup software, sync clients, and bulk file operations trigger false positives — corroborate, don't assume either way).
   - Confirmed-malicious → the machine gets full EDR-incident handling (isolate per the desk's tooling, hands-on via get_ninjaone_device_link); credential exposure → compromised-account-containment. And immediately verify the machine's backups: last clean restore point BEFORE the detection time is the recovery floor — record it in the note.
   - Tamper/self-defense alerts (something tried to stop the agent or delete backups) with no matching maintenance record → hostile until explained; ransomware kills backup agents first.
3. Backup path — run backup-failure-triage with the standard taxonomy (snapshot/VSS on the source, credentials, destination storage/quota, network, agent version) and its recurrence rule. Validation failures count as "restore in doubt," not warnings.
4. Cross-check the two streams both ways — this is the combined product's one real gift: a backup failure preceded by a security detection is potential sabotage, not coincidence; a security detection means recent restore points need a clean/dirty judgment before anyone restores from them. State the cross-check result in the note.
5. End backup-path notes with the exposure statement (last successful backup, validation status); end security-path notes with verdict, actions taken by the product vs the technician, and the last clean restore point. Classify per soc-classification-tree for security events; defensive-writing-standard for anything client-facing.

## Guardrails

- Never triage an Active Protection detection as backup noise — misclassification here is the failure mode this skill exists to prevent.
- Reverted-files ≠ resolved: the process and its persistence outlive the revert; "blocked" is not "done."
- After any security detection, restore points spanning the infection window are suspect — mark them, and never recommend restoring from a possibly-dirty point without a technician's clean/dirty assessment.
- Backup-path guardrails inherit backup-failure-triage in full: no data-safety claims, alerts are evidence, recurring failures never close as one-offs.
- Exclusion requests for Active Protection false positives follow exclusion discipline: confirmed FP evidence, narrowest scope, named approver, review date.
- Console actions (quarantine management, recovery, exclusions) are technician steps the agent directs and records; the agent cannot run scripts via the RMM.
