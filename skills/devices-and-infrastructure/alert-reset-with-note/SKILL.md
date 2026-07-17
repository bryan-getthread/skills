---
name: Alert Reset With Note
description: Reset a NinjaOne alert only after verifying the condition is genuinely healthy again, with an explanation note posted first. Use for "reset this alert" or "clear the disk alert on <device>", attended or embedded in a Flow.
category: Devices & Infrastructure
tools: [list_ninjaone_alerts, get_ninjaone_device, reset_ninjaone_alert, add_ticket_note]
connectors: [NinjaOne]
---

# Alert Reset With Note

**When to use:** "Clear/reset the alert on <device>, it's fine now" — or a stale alert whose condition has visibly recovered.

## Prompt

```
Close out a recovered RMM alert the safe way: verify healthy, write it down, then reset — strictly in that order. Requires NinjaOne; if it is not enabled, say the alert queue is unavailable and stop.

1. Identify the exact alert with list_ninjaone_alerts — device, alert type, message. If several match the description, list them and confirm which one; never reset on a fuzzy match.
2. Allowlist check. Eligible for reset here: device-offline (device back online), disk-space (space recovered), stopped-service (service running again), performance/CPU/memory threshold (readings back in range). NEVER reset and always leave for a human: security/EDR, backup failures, RAID/SMART/hardware-health, anything on a domain controller or hypervisor, and any type you cannot map to this allowlist. The allowlist is a ceiling, not a suggestion.
3. Verify healthy NOW with get_ninjaone_device (resolve org->device without stopping to ask mid-lookup; do not trust the node_class filter — confirm class in details). Offline alert -> device online with fresh last-contact; disk -> free space above threshold; service -> running. Alert text saying "recovered" is not verification; read current state. If verification fails or is inconclusive (device unreachable, stale data), do nothing and say why.
4. Recurrence check: if this same alert fired and was reset 3+ times in 30 days, do not silently reset — flag it as a chronic condition needing a root-cause ticket, and only reset if the requester still wants it after seeing that.
5. Post the explanation note FIRST via add_ticket_note (plain text, no markdown/emojis): which alert, which device, the verified healthy state, and why it is being reset. No note, no reset.
6. Then call reset_ninjaone_alert. Resetting never closes the ticket — closure is a separate human decision.
7. Confirm in your reply: alert cleared, note posted, recurrence status if relevant.

Unattended (Flow) mode: your entire reply is posted verbatim as the plain-text note. Gates, all required: type on the allowlist AND current state verifies healthy AND recurrence under 3-in-30-days. Any gate fails or data is stale/capped -> do nothing, output one plain-text line stating why. Never sweep more than the single alert the Flow handed you.
```
