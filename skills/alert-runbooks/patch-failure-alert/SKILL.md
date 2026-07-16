---
name: Patch Failure Alert
description: Triage a patch/update-failure alert — separate a one-off failure that the next cycle will fix from a repeat offender, detect reboot-pending as the usual culprit, and correlate against the device's patch window. Use when a patch-failed or update-failed alert lands, from any patch engine.
category: Alert Runbooks
tools: [search_tickets, search_ninjaone_devices, get_ninjaone_device, get_ninjaone_device_activities, list_ninjaone_alerts, get_ninjaone_device_link, add_ticket_note, update_ticket]
---

# Patch Failure Alert

Most patch-failure alerts fix themselves on the next cycle; a minority are the same patch failing for the third month while the device drifts out of compliance. This skill tells them apart and routes only the ones that need hands.

## When to use

- A "patch failed / update installation failed" alert opens a ticket.
- "Why does <device> keep failing updates?"
- A Flow sweeps patch-failure alerts after each patch window (see Unattended variant).

## Steps

1. Parse the alert anatomy: device, patch/KB identifier or update name, error code if present, and when the attempt ran. Keep the error code verbatim — it is the single most diagnostic token (do not paraphrase it).
2. Dedupe/recurrence via `search_tickets`: same device + patch failure, past 30–90 days. The verdict differs completely: SAME patch failing repeatedly → stuck patch (corrupt update cache, insufficient space, incompatibility) = needs-tech; DIFFERENT patches failing each cycle → systemic device problem (disk, component store, agent) = needs-tech with a broader brief; first failure ever → likely one-off.
3. Reboot-pending detection — the most common cause: check `get_ninjaone_device` state and `get_ninjaone_device_activities` for a pending-reboot flag, an installed-awaiting-restart event, or a long uptime with recent patch activity. A device that has not rebooted since the last patch run frequently cannot take the next one. If pending reboot on a workstation → the fix is a restart (see Reboot Request Workflow); on a server → needs a maintenance window (see Server Patch Windows).
4. Patch-window correlation: did the attempt run inside the device's designated window? An attempt outside the window (device was awake at an odd hour) failing because a user was active or the device slept mid-install is scheduling noise, not a patch problem. Check activities for shutdown/sleep events mid-install.
5. Verify current state: has the same patch since succeeded? A later successful install of the same KB in activities or alert history (`list_ninjaone_alerts`) means self-healed.
6. Classify and act:
   - Self-healed: the patch subsequently installed → close with a note naming the successful install evidence.
   - Needs-tech: repeat failure of same patch, systemic multi-patch failure, or disk-space/component errors → route with the error code, recurrence history, and `get_ninjaone_device_link`. Remediation (cache reset, manual install) is hands-on — no script execution from here.
   - Needs-client: the failure is on a device the client declined to reboot or excluded from patching per documentation → route to account owner.
   - Noise: attempt interrupted by shutdown/sleep outside the window with no repeat pattern → close as scheduling artifact, note it, and recommend a window review only if it recurs.
7. Output/post: verdict, error code, recurrence class (one-off / stuck patch / systemic), reboot-pending status, window correlation, and route — plain-text note via `add_ticket_note`.

## Guardrails

- Never mark a device patched or compliant from an absence of alerts — only report the install evidence you can see.
- Do not trigger reboots from this skill; a reboot to unblock patching goes through the reboot/maintenance workflows with their confirmations.
- A security-critical patch failing repeatedly is an exposure: say which KB and for how long the device has been unpatched.
- If the patch engine runs outside the RMM (separate console), say the view is partial and name what to check in that console.
- Plain-text notes only; keep error codes verbatim.

## Unattended (Flows) variant

- Your entire reply is posted verbatim as the ticket note — plain text, no narration.
- Deterministic gates: close ONLY when a subsequent successful install of the same patch is evidenced. Reboot-pending workstation → route to the reboot-request queue; reboot-pending server → route to the patch-window/maintenance queue; recurrence (same patch ≥2 prior failures, or ≥3 mixed failures in 90 days) → route to tech queue as stuck/systemic; everything else → route as one-off for next-cycle watch, do not close.
- Unverifiable state or result-capped searches → route to a human with the cap noted.
