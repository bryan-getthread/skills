---
name: Liongard Sophos Firewall Read
description: Interrogate a client's Sophos Firewall (XG/SFOS) through the Liongard Sophos Firewall inspector — firmware, firewall rules, port-forwards/NAT, VPN config, interfaces, admin access. Use for "what's their Sophos firewall config?", rule/exposure review during triage, or firmware-currency checks.
category: Liongard Inspectors
tools: [liongard_environment, liongard_launchpoint, liongard_metric, liongard_query, liongard_detection, liongard_timeline, search_tickets, search_itglue, search_hudu, add_ticket_note]
---

# Liongard Sophos Firewall Read

Reads Sophos Firewall (SFOS / XG) config — firmware, interfaces, firewall rules, NAT/port-forwards, VPN tunnels, and admin access — from the Liongard Sophos Firewall inspector's dataprint, so a tech gets the edge picture during triage without logging into the appliance.

## When to use

- "What firewall rules / port-forwards are configured at <client>?"
- Connectivity or exposure ticket needs the rule and NAT picture before anyone touches the firewall.
- "What firmware is their Sophos firewall on?" — currency / vulnerability check.
- "What VPN tunnels are configured?" — remote-access or site-to-site troubleshooting.
- Post-change incident: "did a firewall rule or NAT entry change?"

## What the inspector captures

Appliance model and firmware, interface/zone layout, firewall rule base, NAT and port-forward entries, VPN (IPsec/SSL) configuration, and administrative access. Exact coverage varies by inspector version.

## Steps

1. Follow liongard-inspectors/liongard-access-pattern: resolve the environment, `liongard_launchpoint` filtered by systemType "Sophos Firewall" / "SFOS" / "XG", verify last-run success, note dataprint age. A client may have multiple firewalls — enumerate them.
2. Pick the angle and query via `liongard_metric` or `liongard_query`. Illustrative JMESPath angles — verify field paths against the live dataprint; schemas vary by inspector version:
   - Rules: firewall rule list with action, source/dest zones, service — flag any-any allows.
   - Port-forwards/NAT: inbound NAT entries with external port → internal host — flag exposure of sensitive services (RDP/3389, SMB/445).
   - Firmware: current SFOS version — compare against vendor's current release.
   - VPN: IPsec/SSL tunnel list with peers and status.
   - Admins: local/remote admin accounts and access methods.
3. For "what changed" questions, pull `liongard_detection` / `liongard_timeline` scoped to the firewall: rule adds/edits, NAT changes, firmware upgrades, admin adds — line them up against the incident window.
4. Sanity-check surprising values (zero rules) — usually a wrong field path or partial inspection. Re-probe broadly.
5. Output: the requested facts in a compact table, source + dataprint age line, and flags (risky inbound NAT, any-any rules, outdated firmware, unexpected admins). Offer a plain-text `add_ticket_note`.

## How this feeds tickets

- **Triage**: attach the rule/NAT facts to a connectivity ticket so the tech starts with the topology.
- **Exposure review**: inbound NAT to RDP/SMB and any-any rules are recurring risk findings — surface them with the as-of date, recommend the tech confirm live.
- **Incident correlation**: an outage minutes after a detected rule or firmware change is a lead — label it time-adjacency, not proven cause.

## Guardrails

- Read-only: this skill never edits firewall config. Changes go through the partner's change process (see devices-and-infrastructure/firewall-rule-change).
- Never paste PSKs, VPN pre-shared keys, or credentials into notes even if the dataprint exposes them — reference "PSK on file in <doc platform>" instead.
- Always state dataprint age; firewall config changes deserve a "verify live" caveat on active incidents.
- If no Sophos Firewall launchpoint exists, degrade per the access pattern: docs → ticket history → "verify on appliance".
- Exposure findings are risk inputs, not confirmed breaches — "external RDP NAT present, verify need" phrasing.
- Plain-text notes only.
