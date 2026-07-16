---
name: Auvik Network Monitoring
description: An Auvik network-monitoring alert landed — separate a device-down from an interface-down from a config-change, use the topology map to see cascades before chasing symptoms, and feed chronic noise into an alert-tuning loop. Verify against Auvik's current documentation.
category: Vendor Runbooks
tools: [search_tickets, search_clients, search_contacts, search_itglue, add_ticket_note, update_ticket]
---

# Auvik Network Monitoring

Vendor runbook for Auvik network-infrastructure monitoring. Auvik's value is the topology map plus config-change detection, and its alerts fall into a few families that demand different responses — the recurring mistake is chasing a downstream symptom when a parent device is the real cause. This skill adds map-driven triage and an alert-tuning loop that hands chronic noise to alert-noise-assessment. Verify feature names and console layout against Auvik's current documentation.

## When to use

- An Auvik alert lands: device down/unreachable, interface down/errors, or a configuration change
- A "the network is down at <site>" report needs structured triage against the topology
- Auvik alert volume is high and someone asks which alerts are noise / what to retune

## Steps

1. Classify the alert family first — they are not the same problem:
   - **Device down / unreachable** — Auvik can't poll the device (SNMP/ICMP). Distinguish a device that is truly down from one that is merely unreachable by Auvik (the collector lost a path, SNMP creds/community changed, or the collector itself is down). Confirm the Auvik **collector** for that site is healthy before trusting a wave of "down" alerts — a dead collector reports the whole site down.
   - **Interface down / interface errors** — a port/link state or error-rate condition. A single access-port flap on an endpoint is minor; an uplink/trunk down between switches is a cascade source. Read which interface on which device.
   - **Configuration change** — Auvik detected a device config change (its config-backup/diff). This is a change-management signal: was it planned/authorized, or unexpected? An unexpected change on a firewall/switch is both an outage-risk and a possible-security signal.
2. Map-driven triage — use topology before chasing symptoms: open the affected device on the Auvik network map and check its upstream parent. If a parent switch/router/firewall is down, the flood of "down" alerts for everything behind it are children of one root cause — work the parent, not each leaf. State the root device and the cascade explicitly so the desk doesn't open ten tickets for one failure. Merge related symptom tickets to the root where appropriate.
3. Respond by family:
   - Device-down with a healthy collector and dead parent-none → the device itself: check for power/site issues, recent change, and whether it was in a maintenance window. Site-wide (parent = the site's edge/ISP) → possible circuit/ISP outage — correlate other devices at the site and check for an ISP advisory.
   - Interface uplink/trunk down → potential partial-network partition; interface errors climbing → cabling/duplex/optics — a degradation, not a clean down.
   - Config-change → verify against change records (search_tickets for a planned change; search_itglue for the client's change policy). Unexpected/unauthorized change on network gear → treat as change-and-problem plus a possible-security event; do not assume benign.
4. Recurrence and context via search_tickets (same device/interface/site, 30–90 days): a link that flaps nightly is a chronic problem ticket, not a nightly close.
5. Alert-tuning loop — chronic noise is a project, not a per-alert annoyance: when a device/interface/site generates repeated self-clearing or benign alerts (flapping access ports, a known-degraded but accepted link, a decommissioned device still polled), route the pattern to alert-noise-assessment for a quantified retune recommendation (threshold, dedup window, auto-close self-healing pairs, or stop polling a retired asset). Recommend the tuning; do not silently suppress a device that could also fail for real.
6. Document: alert family, root vs symptom (with the topology cascade), verdict, recurrence, and — for config changes — authorized-or-not. Console/monitoring changes are technician actions the agent directs and records.

## Guardrails

- Confirm the site collector is healthy before trusting a wave of device-down alerts — a dead collector fakes a site-wide outage.
- Always check the topology parent before working a device-down as its own incident — one dead parent explains a flood of children; work the root, merge the symptoms.
- An unexpected/unauthorized config change is both an outage-risk and a possible-security signal — never assume benign; verify against change records.
- A flapping/recurring alert is a chronic problem ticket or a tuning candidate, never a repeated one-off close.
- Alert tuning is a recommendation routed through alert-noise-assessment — never suppress or disable monitoring silently, and prefer threshold changes over removing a check that could catch a real failure.
- The agent has no Auvik console access — polling/collector/config actions are technician steps it directs and records; degradation — without documentation access (search_itglue), the client's network topology intent and change policy may be unknown, so state what the tech should confirm on the map.
