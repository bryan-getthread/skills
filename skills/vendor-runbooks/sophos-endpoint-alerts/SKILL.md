---
name: Sophos Endpoint Alerts
description: A Sophos Central endpoint alert needs triage — read the health status and cleanup result, handle tamper protection correctly, and verify cleanup actually completed before closing.
category: Vendor Runbooks
tools: [search_tickets, search_ninjaone_devices, get_ninjaone_device, get_ninjaone_device_activities, get_ninjaone_device_link, add_ticket_note, update_ticket]
---

# Sophos Endpoint Alerts

Vendor specialization of edr-detection-runbook for Sophos Central endpoints. Adds the Sophos-specific reads: health-status colors, automatic-cleanup vs manual-cleanup-required alerts, and the tamper-protection step that trips up half of all hands-on remediation. Verify console layout against Sophos's current documentation.

## When to use

- A Sophos Central alert lands as a ticket (malware detected/cleaned, PUA, manual cleanup required, device health red/yellow)
- A tech is about to reinstall/repair a Sophos agent and hits tamper protection
- Someone asks whether a Sophos "cleaned up" alert can just be closed

## Steps

1. Parse the alert anatomy: alert type and severity, hostname, threat name/path, and — the key Sophos field — the cleanup outcome: "cleaned up" (automatic cleanup succeeded), "manual cleanup required" (it didn't), or "running/pending." Also note the device's health status color (green/yellow/red) in Sophos Central, which aggregates protection state, pending reboots, and active threats.
2. Device and user context per edr-detection-runbook: get_ninjaone_device for role and assigned user, get_ninjaone_device_activities around the detection, user corroboration via a verified channel.
3. Branch by cleanup outcome:
   - Cleaned up automatically → verify, don't assume: check the device health returned to green and no repeat detections followed. A cleaned commodity-malware hit with corroborated context is a close-with-evidence path; repeated cleanups of the same threat on the same device mean an uncleaned source (persistence, network share, USB, browser sync) — keep it open and scope.
   - Manual cleanup required → this is Sophos saying automation failed; treat the threat as present. Technician remediates hands-on via get_ninjaone_device_link; on servers or anything with credential exposure, branch to compromised-account-containment for signed-in users.
   - PUA detections → judgment class: corroborate with the user/business context (remote-access tools and crack-adjacent utilities are the common cases); authorize-or-remove is a documented decision, not a silent allow.
4. Tamper protection discipline: any repair, reinstall, or removal of the agent requires disabling tamper protection for that device from Sophos Central first (technician portal action) — never by hunting for workarounds on the endpoint. Re-enable it immediately after the maintenance window and record both timestamps. A tamper-protection alert (someone attempted to disable protection) with no matching maintenance record is treated as hostile until explained.
5. Isolation: for active/spreading threats, the technician isolates the device from Sophos Central; release only after cleanup verification (rescan clean, health green, persistence rechecked).
6. Health-status cleanup: red/yellow health alerts without a threat (outdated definitions, service stopped, reboot required) are hygiene work — fix the cause, don't suppress the signal; recurring fleet-wide health noise routes to security-noise-tuning or patch-compliance-review as appropriate.
7. Document the decision, not just the action — cleanup outcome, verification evidence, tamper-protection windows — and classify per soc-classification-tree. Client-facing wording per defensive-writing-standard.

## Guardrails

- "Cleaned up" is a claim, not a verdict — verify health status and absence of repeats before closing; repeated cleanups of the same threat are never closed as one-offs.
- Tamper protection is never left disabled after maintenance — re-enable and record it; an unexplained tamper-disable attempt is a security event, not noise.
- Exclusion/allow requests (PUA authorizations included) follow the exclusion discipline: narrowest scope, named approver, review date — convenience is not a justification.
- The agent cannot run scripts or deploy software via the RMM; console actions are technician steps the agent directs and records.
- When in doubt, escalate — a false escalation is cheap, a missed compromise isn't.
- Degradation: no RMM connected → work from the alert body and ticket history; state the reduced visibility.
