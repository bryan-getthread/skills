---
name: AV/EDR Agent Offline Alert
description: Triage an "endpoint protection agent not reporting" alert from any AV/EDR product — establish whether the device is off or the device is up with a dead agent, quantify how long the endpoint has been unprotected, and route with that protection-gap framing. Use for agent-offline, agent-not-checked-in, or sensor-unhealthy alerts.
category: Alert Runbooks
tools: [search_tickets, search_ninjaone_devices, get_ninjaone_device, get_ninjaone_device_activities, list_ninjaone_alerts, get_ninjaone_device_link, add_ticket_note, update_ticket]
---

# AV/EDR Agent Offline Alert

An AV/EDR agent that stopped reporting means one of two very different things: the whole device is off (usually benign), or the device is running with its protection dead (a live gap). This skill makes that distinction explicit and frames the ticket around unprotected time, not agent plumbing.

## When to use

- "Agent offline / sensor not checked in / endpoint unhealthy" alerts from any endpoint-protection product.
- "Is <device> actually unprotected or just powered off?"
- A Flow sweeps agent-offline alerts and needs deterministic routing (see Unattended variant).

## Steps

1. Parse the alert anatomy: device name, product, last check-in timestamp, and alert age. The last check-in is the start of the potential protection gap — carry it through the whole triage.
2. Dedupe/recurrence via `search_tickets`: same device + agent-offline in 30 days. A device whose agent repeatedly drops and returns is a broken-agent pattern (conflicting software, failed updates, tamper) — that is a reinstall/repair ticket, not a series of one-offs.
3. The pivotal check — is the DEVICE online? Pull `get_ninjaone_device` (resolve via `search_ninjaone_devices`):
   - Device offline too, last RMM contact ≈ agent last check-in → the machine is powered off or off-network. No active gap while it stays off; the gap begins when it powers on. Route as a device-offline follow-up (see Device Offline Runbook), not a security incident.
   - Device ONLINE with fresh RMM contact but agent silent → device is running unprotected right now. This is the real case: treat with urgency proportional to the gap duration.
4. For the online-but-agent-dead case, gather evidence: `get_ninjaone_device_activities` around the last check-in (agent update, application install, user action, reboot that the service never survived); `list_ninjaone_alerts` for related service-stopped alerts.
5. Classify:
   - Self-healed: the protection console shows the agent has since checked in (alert text or a newer recovery event) AND device state is consistent → close with a note stating the gap window (from–to).
   - Needs-tech: device online, agent dead → route as a protection gap with the duration ("unprotected since <timestamp>, N hours"), evidence, and the `get_ninjaone_device_link` deep link. Agent repair/reinstall is hands-on or console work — this integration cannot push software.
   - Needs-client: device is a personal/BYOD or out-of-scope machine per documentation → route to account owner for scope decision.
   - Noise: device decommissioned or offboarded but never removed from the protection console → route as hygiene (remove from console), never auto-close, because "decommissioned" must be verified by a human.
6. Output/post: verdict, device state vs agent state, unprotected duration, recurrence history, and route — plain-text note via `add_ticket_note`. Where the endpoint had a live gap, include an exposure statement: the window during which the endpoint was online without protection.

## Guardrails

- Never close an agent-offline alert on the assumption the device is off — verify device state; "probably powered down" is not evidence.
- A running device with dead protection is never noise, whatever the recurrence count.
- Do not attempt agent reinstalls or service manipulation from here; tamper-protected agents fail service restarts by design. Hand off with the deep link.
- If the RMM is not enabled, say the device-state check is unavailable and route everything that is not provably recovered to a human.
- Plain-text notes only. Never state the device "is protected" — only report last check-in times.

## Unattended (Flows) variant

- Your entire reply is posted verbatim as the ticket note — plain text, no narration.
- Deterministic gates: close ONLY when a recovery event exists (agent checked in after the alert) AND device state is consistent with it. Device online + agent silent → always route to the security/tech queue with the gap duration; device and agent both offline → route to the device-offline queue. Recurrence ≥3 in 30 days → route as broken-agent pattern.
- Anything unverifiable (RMM absent, device not found, stale data) → route to a human, never close.
