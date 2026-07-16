---
name: SD-WAN / Multi-Circuit Monitoring
description: Review a multi-circuit or SD-WAN site's connectivity — confirm each circuit is up, failover actually works, and open the right ISP escalation when a circuit is down or degraded. Use when a site has multiple WAN links, someone reports one circuit down, or you're verifying failover posture.
category: Devices & Infrastructure
tools: [liongard_launchpoint, liongard_metric, liongard_timeline, search_ninjaone_devices, search_itglue, web_search, create_ticket, add_ticket_note]
---

# SD-WAN / Multi-Circuit Monitoring

For a site with more than one WAN link, confirm the state of each circuit, prove that failover is real (not just configured), and drive a clean ISP escalation when a circuit is down or degraded.

## When to use

- "Is <site>'s backup circuit actually working?" / failover verification.
- One circuit at a multi-WAN or SD-WAN site is reported down or flapping.
- Post-outage review of how failover behaved.
- Confirming a new secondary circuit is live and in the failover policy.

## Steps

1. Establish the site's circuit topology: how many circuits, primary vs. secondary, carrier and account per circuit, and the SD-WAN/edge platform. Pull the documented circuit inventory (`search_itglue`) — cross-reference the circuit-inventory skill — and the edge device from the RMM (`search_ninjaone_devices`).
2. Read current state per circuit. Where Liongard runs, `liongard_launchpoint` filtered by the SD-WAN/firewall systemType (e.g. "Meraki", "Fortinet", "VeloCloud", "Palo Alto") gives the edge inspector; `liongard_metric` reads per-uplink status, and `liongard_timeline` shows recent uplink up/down transitions. Confirm the inspector last ran and state dataprint age; where none exists, degrade to RMM reachability and documentation and say the check is partial.
3. Verify failover, not just presence: confirm the secondary circuit is up AND is configured in the failover/SD-WAN policy AND has recent evidence of carrying or being able to carry traffic. A secondary that is "up" but not in the policy is a silent single-point-of-failure — flag it.
4. For a down/degraded circuit, assemble the ISP escalation package: carrier, account/circuit ID, site address and demarc, the observed symptom with timestamps (from the uplink transition history), and what's already been checked. Confirm current carrier support contact/process via documentation or `web_search` — do not guess a support path.
5. Classify the site: Healthy (all circuits up, failover verified), Degraded (running on one circuit / failover unverified), or Down. State which circuit is carrying traffic now.
6. Output a status summary per circuit plus the failover verdict and, if needed, the ready-to-send ISP escalation package. Post to the ticket as a plain-text note (`add_ticket_note`) or `create_ticket`. For a broader single-circuit outage, hand off to the isp-outage-tracking skill.

## Guardrails

- "Circuit is up" is not "failover works" — do not report a site as resilient without confirming the secondary is in the failover policy and viable.
- Trust Liongard/RMM data only after confirming the inspector/agent last ran; report dataprint age. Circuit transition timestamps come from the timeline/inspector, not from assumption.
- Verify carrier support contacts/process from documentation or current sources before escalating — never invent an ISP ticket number, phone number, or portal.
- This skill reads state and prepares escalations; it does not fail traffic over, change SD-WAN policy, or contact the carrier on its own.
- Notes posted to tickets are plain text — no markdown, no emojis.
