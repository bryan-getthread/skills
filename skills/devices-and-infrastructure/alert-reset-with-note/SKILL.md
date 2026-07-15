---
name: Alert Reset With Note
description: Reset a NinjaOne alert only after verifying the underlying condition is actually healthy again, and always with an explanation note posted first. Use when clearing recovered alerts — "reset this alert", "clear the disk alert on <device>" — attended or embedded in a Flow.
category: Devices & Infrastructure
tools: [list_ninjaone_alerts, get_ninjaone_device, reset_ninjaone_alert, add_ticket_note]
---

# Alert Reset With Note

Closes the loop on a recovered RMM alert the safe way: verify the condition is genuinely healthy, write the explanation down, then reset — in that order.

## When to use

- "Clear/reset the alert on <device>, it's fine now."
- An alert's condition has visibly recovered (device back online, disk freed) and the alert is stale.
- A Flow wants recovered alerts swept automatically (see Unattended variant).

## Steps

1. Identify the exact alert via `list_ninjaone_alerts` — device, alert type, and message. If several alerts match the description, list them and confirm which one; never reset on a fuzzy match.
2. Check the alert type against the non-critical allowlist. Eligible for reset by this skill: device-offline (device back online), disk-space (space recovered), stopped-service (service running again), performance/CPU/memory threshold (readings back in range). NOT eligible — always leave for a human: security/EDR alerts, backup failures, RAID/SMART/hardware-health, anything on a domain controller or hypervisor, and any alert type you cannot map onto the allowlist.
3. Verify the state is actually healthy NOW with `get_ninjaone_device`: for an offline alert the device must be online with a fresh last-contact; for disk the free space must be above threshold; for a service alert the service must be running. The alert text saying "recovered" is not verification — read the current state.
4. Recurrence check: if this same alert has fired and been reset repeatedly (3+ times in 30 days), do not silently reset again — flag it as a chronic condition that needs a root-cause ticket, and only reset if the requester still wants it after seeing that.
5. Post the explanation note FIRST via `add_ticket_note` (plain text): which alert, on which device, what the verified healthy state is, and why it is being reset. The note is the audit trail — no note, no reset.
6. Then call `reset_ninjaone_alert`.
7. Confirm the reset in your reply: alert cleared, note posted, and the recurrence status if relevant.

## Guardrails

- Verify-then-note-then-reset, strictly in that order. If verification fails or is inconclusive (device unreachable, stale data), do nothing and say why.
- The allowlist is a ceiling, not a suggestion — unknown alert types are never reset.
- Never reset an alert to make a queue look clean; the recurrence check exists to stop alert-blindness.
- Resetting an alert never closes the ticket — ticket closure is a separate, human-owned decision.
- If NinjaOne is not enabled, this skill cannot run; say so.

## Unattended (Flows) variant

- Your entire reply is posted verbatim as the ticket note — no narration, no markdown, no emojis. Write it as the plain-text explanation note described in step 5.
- Deterministic gates: alert type must be on the allowlist AND current device state must verify healthy AND recurrence must be under 3 in 30 days. All three or no action.
- When any gate fails, in doubt, or data is stale/capped: do nothing and output a one-line plain-text reason ("Alert not reset: current state could not be verified as healthy.").
- Never reset more than the single alert the Flow handed you; no sweeping in unattended mode.
