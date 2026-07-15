---
name: Reboot Request Workflow
description: Reboot a device through the RMM with user approval — confirm the user is logged off or has saved work, choose normal vs forced deliberately, and verify the device comes back. Use for "reboot <device>", pending-reboot remediation, or "this needs a restart to finish patching".
category: Devices & Infrastructure
tools: [search_ninjaone_devices, get_ninjaone_device, reboot_ninjaone_device, add_ticket_note]
---

# Reboot Request Workflow

Runs a reboot as a small change, not a reflex: user coordination first, deliberate normal-vs-forced choice, and post-reboot verification that the machine actually returned.

## When to use

- "Reboot <user>'s machine" / "restart <device> to clear the pending reboot."
- A diagnosis (health check, patch review) concluded a reboot is the fix.
- Patching is stuck behind a restart and the user has agreed to a window.

## Steps

1. Resolve organization then device via `search_ninjaone_devices`; rank by org match then last-contact; verify class in `get_ninjaone_device` details. A server reboot is a different decision than a workstation reboot — route servers through explicit confirmation of the blast radius (dependent users/services) every time.
2. Check who is affected: last-logged-on user and current online state from device details, and whether another tech is actively working the device (recent remote sessions in activities). If a tech is on it, coordinate first.
3. User approval is the gate: confirm the user is logged off, or has been contacted and has saved their work and agreed to a time. "The ticket says reboot" is not user approval. If the requester IS the user, confirm they are ready now. Record who approved and when.
4. Choose the reboot type deliberately:
   - Normal reboot (default): applications get the chance to close cleanly; use whenever a user session may exist.
   - Forced reboot: only when the machine is hung/unresponsive to a normal reboot, or confirmed to have no active session — forced closes discard unsaved work. Never force as the first attempt on a machine with a logged-in user.
5. Execute via `reboot_ninjaone_device`, then post the plain-text note via `add_ticket_note`: device, reason, approval (who/when), reboot type, and time issued.
6. Post-reboot verification: poll `get_ninjaone_device` until the device reports back online (allow several minutes; servers longer). Confirm last-contact refreshed, and where relevant re-check the original condition (pending-reboot flag cleared, patch completed). If the device does not return within a reasonable window (~15 minutes workstation, ~30 server), treat it as an incident: switch to the Device Offline Runbook and tell the requester immediately — a machine that never came back is the worst outcome of this workflow and must not go unnoticed.
7. Report the outcome: back online at <time>, original condition status, note posted.

## Guardrails

- No reboot without user coordination, ever — the confirm-before-changes-affecting-a-user rule is the core of this skill.
- Servers: additionally confirm timing against business hours and dependent services; recommend a maintenance window for anything shared.
- Forced reboots require explicit acknowledgment that unsaved work will be lost.
- Never fire-and-forget: the workflow is not done until the device is verified back online or the failure is escalated.
- If NinjaOne is not enabled, say remote reboot is unavailable and provide user-guided restart instructions instead.
- Plain-text notes only.
