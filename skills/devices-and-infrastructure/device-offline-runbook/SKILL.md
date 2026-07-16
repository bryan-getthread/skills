---
name: Device Offline Runbook
description: Work a device-offline alert or "computer won't connect" ticket through a structured diagnostic — site-wide check first, maintenance windows, last activities, power/on-site guidance, and clear escalate criteria. Use when a device shows offline in the RMM or an offline alert fires.
category: Devices & Infrastructure
tools: [search_ninjaone_devices, get_ninjaone_device, get_ninjaone_device_activities, list_ninjaone_alerts, search_itglue, add_ticket_note]
---

# Device Offline Runbook

The 10-step offline diagnostic: rule out the big causes first (site outage, maintenance, decommission), reconstruct what happened from activities, then give the tech precise on-site guidance or an escalation call.

## When to use

- A device-offline alert lands on the board.
- "Why is <device> offline?" / "<user> says their machine won't connect."
- An offline device turned up in a fleet sweep and needs a verdict.

## Steps

1. Resolve the device: organization first, then `search_ninjaone_devices`. Rank candidates by organization match then most recent last-contact; state your pick. Verify the device class in details — do not trust the node_class filter.
2. Site-wide check FIRST: pull the organization's other devices. If several devices at the same site dropped around the same time, this is a network/site outage, not a device problem — switch to the Network Outage Triage skill and say so.
3. Check maintenance: is the device in a maintenance window, or was maintenance recently set? A device offline inside planned maintenance is not an incident — note it and stop unless the window has expired.
4. Establish the timeline: `get_ninjaone_device` for exact last-contact time; `get_ninjaone_device_activities` for what happened just before it dropped (shutdown event, reboot initiated, agent update, user logoff, patch install).
5. Check alert history via `list_ninjaone_alerts`: is this device a flapper (repeated offline/online pairs)? Flapping suggests NIC/power/Wi-Fi issues, not a hard failure.
6. Classify the likely cause from the evidence: clean shutdown before drop → powered off (laptop closed, user left); patch/reboot activity before drop → hung during restart; no preceding activity → power loss, network path failure, or hardware; last contact weeks ago → possibly retired or off-network long-term.
7. Cross-check documentation (`search_itglue`) and recent tickets: was this device scheduled for replacement, reimage, or decommission?
8. Check whether another tech is already on it — recent remote-session or manual activities mean coordinate, don't duplicate.
9. If it looks like a local/power issue, give the requester or on-site contact concrete physical checks: power/LED state, network cable/Wi-Fi association, whether the machine responds locally, power-cycle guidance. The agent cannot see a powered-off machine — say what only on-site eyes can verify.
10. Escalate when: it is a server or shared infrastructure device; it hosts services others depend on; it has been offline past the client's tolerance with no on-site explanation; or physical checks fail. Escalation output: timeline, evidence, checks already done, and what the next tier should try. Offer to post the diagnostic as a plain-text note via `add_ticket_note`.

## Guardrails

- Never mark an offline alert resolved just because the device came back — confirm stability (no immediate re-drop) before closing anything, and closing tickets is the requester's call.
- Do not claim a cause you cannot evidence; distinguish "confirmed" (activity log shows shutdown) from "likely" (no data).
- Long-offline devices are flagged as "verify still in service" — do not treat possible retirements as incidents.
- If NinjaOne is not enabled, degrade to ticket history + documentation and say the RMM view is unavailable.
- Plain-text notes only when posting to the ticket.

## Unattended (Flows) variant

- Follows the Unattended Output Discipline contract: the entire reply is the plain-text diagnostic note posted verbatim — site-wide check result, maintenance status, last-contact timeline, likely cause labeled confirmed vs likely, and the on-site checks or escalation call. No narration.
- Deterministic inputs from the flow: the device id from the triggering alert (never resolved by name-guessing unattended). Device unresolvable → output nothing.
- Deterministic branches inside the note: multiple devices at the site dropped together → the note leads with `PROBABLE SITE OUTAGE - not a device issue` and skips the per-device diagnostic; device inside an active maintenance window → the note is exactly `IN MAINTENANCE WINDOW - NO INCIDENT.`
- Permitted writes: `add_ticket_note` only. Never resolve or close the alert or ticket — a device coming back online is not confirmed stability, and closing is a human call.
- NinjaOne not enabled → output nothing.
