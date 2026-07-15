---
name: Network Outage Triage
description: Triage a suspected site-down — distinguish the all-devices-offline pattern from a single dead device, split ISP from internal causes, identify who to call, and set a comms cadence. Use when multiple offline alerts fire from one client or someone reports "the whole office is down".
category: Devices & Infrastructure
tools: [search_ninjaone_devices, list_ninjaone_alerts, get_ninjaone_device, search_itglue, search_tickets, search_contacts, merge_ticket, add_ticket_note]
---

# Network Outage Triage

Reads the offline pattern across a client's fleet to call site-down vs single-device, points at ISP vs internal infrastructure, and sets up the two things an outage needs: the right phone calls and a communication rhythm.

## When to use

- A burst of offline alerts from one client hits the board.
- "<client> says nobody can get online / the whole office is down."
- A single offline ticket smells bigger — the Device Offline Runbook's site-wide check routed here.

## Steps

1. Establish the pattern first: pull the client's devices (`search_ninjaone_devices`) and current alerts (`list_ninjaone_alerts`). All or most devices at a site dropping within minutes of each other = site event. A subset sharing a closet/switch/VLAN (infer from naming and IP ranges) = internal segment. One device = not an outage; route back to the Device Offline Runbook. Flag result caps — an undercounted fleet can make a full outage look partial.
2. Note the drop time from last-contact timestamps — a tight cluster is a hard cut (power, circuit, core switch); a stagger suggests something degrading (DHCP exhaustion, spanning-tree, failing hardware).
3. ISP vs internal, from outside evidence: if the firewall/router/edge device is itself unreachable but devices with independent connectivity (LTE-connected, other site) are fine, the cut is at or upstream of the edge. If the edge answers but everything behind it is dark, it is internal (switch, power to the closet, DHCP). Say which evidence supports the call — from the RMM's outside view this is inference, and on-site confirmation beats it.
4. Pull the who-to-call sheet from documentation (`search_itglue`): ISP name, account/circuit numbers, support line; on-site contact; and the site's network gear inventory. If the docs are missing these, say so — that is a finding, not a footnote.
5. Check scope beyond this client: are other clients on the same ISP/region also dropping (`list_ninjaone_alerts` across organizations)? A regional ISP event changes the message and the fix.
6. Consolidate the ticket noise: identify the offline-alert tickets this event spawned (`search_tickets`), designate one master incident ticket, and offer to `merge_ticket` the duplicates into it — with the requester's confirmation.
7. Set the comms cadence and write the first update: who has been called (ISP with circuit ID / on-site contact walking to the closet), what is confirmed vs suspected, next update time. Recommended rhythm: first client update immediately, then every 30 minutes for a full outage (hourly for partial) until restored, plus a closing summary. Post updates as plain-text notes via `add_ticket_note`.
8. On restoration: verify devices actually re-report online (spot-check last-contact), note the outage window and cause as confirmed/unconfirmed, and recommend a follow-up if cause is unknown — unexplained outages repeat.

## Guardrails

- Never declare ISP fault to the client without either ISP confirmation or edge-unreachable evidence — "we are investigating with the carrier" until confirmed. Defensive writing; no speculation in client-visible updates.
- Merges only with confirmation, and only tickets verifiably from this event (same client, same window, offline-type) — never merge on title similarity.
- The RMM sees the site from outside; every internal-topology conclusion is labeled as inference until someone on-site confirms.
- Do not reset offline alerts during the event — they are the telemetry. Alert cleanup happens after restoration via the Alert Reset skill's rules.
- Missing ISP/contact documentation gets flagged for post-incident remediation.
- If NinjaOne is not enabled, triage from ticket volume and documentation alone and say the live fleet view is missing.
- Plain-text notes only.
