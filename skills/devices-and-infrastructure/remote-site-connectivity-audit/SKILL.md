---
name: Remote Site Connectivity Audit
description: Review the connectivity posture of a branch office or remote-worker population — circuits, edge gear, VPN/tunnels, and last-mile resilience — and surface single points of failure and gaps. Use when someone asks to audit a branch/remote site's connectivity, review remote-worker access, or assess WAN resilience.
category: Devices & Infrastructure
tools: [liongard_launchpoint, liongard_metric, liongard_timeline, search_ninjaone_devices, get_ninjaone_device, search_itglue, create_ticket, add_ticket_note]
---

# Remote Site Connectivity Audit

Give a branch office or a remote-worker population an honest connectivity posture: what links and tunnels exist, where the single points of failure are, and which gaps need fixing — before an outage finds them for you.

## When to use

- "Audit connectivity for the <branch> office."
- Reviewing remote-worker / home-office access resilience.
- Onboarding a new site and confirming its connectivity is sound.
- WAN/VPN resilience review across a client's locations.

## Steps

1. Scope the audit: a specific branch, all branches, or the remote-worker population. Pull each site's documented connectivity (`search_itglue`) — circuits, edge device, VPN/SD-WAN topology — and the edge/endpoint devices from the RMM (`search_ninjaone_devices`, `get_ninjaone_device` for reachability and last contact).
2. For each site, read the connectivity stack. Where Liongard runs, use `liongard_launchpoint` (firewall/SD-WAN/edge systemType) with `liongard_metric` and `liongard_timeline` to read uplink status, tunnel/VPN state, and recent up/down events. Confirm the inspector last ran and state dataprint age; degrade to RMM reachability + documentation where absent and say the audit is partial.
3. Assess resilience per site and flag single points of failure:
   - WAN: single circuit vs. redundant, and whether failover is real (hand a multi-circuit/SD-WAN site to the sd-wan-circuit-monitoring skill; a single-circuit outage to isp-outage-tracking; cross-reference circuit-inventory for the circuit records).
   - Edge: single firewall/router with no spare vs. HA pair; firmware currency.
   - Remote access: VPN/tunnel health, split-tunnel vs. full, MFA on the access path, and whether the tunnel is monitored.
   - Power/last-mile: UPS behind the edge gear (cross-reference power-ups-capacity-planning), and any known last-mile fragility.
4. For remote workers, review the access posture rather than a physical circuit: managed vs. unmanaged device, VPN/ZTNA in use, MFA enforced, and whether the endpoint is checking into the RMM.
5. Rank findings by risk (single circuit at a business-critical site, unmonitored tunnel, edge with no HA/spare, MFA gap) and pair each with a concrete remediation and owner.
6. Output a posture report per site/population: what exists, the resilience verdict, the ranked SPOFs/gaps with remediations. Post to the ticket as a plain-text note (`add_ticket_note`) or `create_ticket` to track remediation.

## Guardrails

- Trust Liongard/RMM data only after confirming the inspector/agent last ran; report dataprint and last-contact ages. Where a site has neither, mark it "documentation-only — unverified".
- Report resilience from evidence, not assumption — do not call a site redundant without confirming failover viability, and do not call a tunnel healthy without a current state signal.
- This skill audits and recommends only; it changes no config, tunnel, or route. Remediation routes to the relevant change skills.
- Do not expose credentials, tunnel secrets, public IPs, or site identifiers in output.
- If any device/site listing may have hit a result cap, report counts as "at least N".
- Notes posted to tickets are plain text — no markdown, no emojis.
