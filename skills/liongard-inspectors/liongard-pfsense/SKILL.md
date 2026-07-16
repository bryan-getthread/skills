---
name: Liongard pfSense Read
description: Interrogate a client's pfSense firewall through the Liongard pfSense inspector — version, interfaces, firewall/NAT rules, VPN config, packages, admin access. Use for "what's their pfSense config?", rule/exposure review during triage, or version-currency checks.
category: Liongard Inspectors
tools: [liongard_environment, liongard_launchpoint, liongard_metric, liongard_query, liongard_detection, liongard_timeline, search_tickets, search_itglue, search_hudu, add_ticket_note]
---

# Liongard pfSense Read

Reads pfSense firewall config — version, interfaces, firewall and NAT rules, VPN config, installed packages, and admin access — from the Liongard pfSense inspector's dataprint, so a tech gets the edge picture during triage without logging into the pfSense web UI.

## When to use

- "What firewall rules / port-forwards are configured at <client>?"
- Connectivity or exposure ticket needs the rule and NAT picture before anyone touches the firewall.
- "What pfSense version are they on?" — currency/vulnerability check.
- "What VPN (IPsec/OpenVPN) is configured?" — remote-access troubleshooting.
- Post-change: "did a firewall rule or NAT entry change?"

## What the inspector captures

pfSense version, interface/assignment layout, firewall rule base, NAT/port-forward entries, VPN (IPsec/OpenVPN) config, installed packages, and administrative access. Exact coverage varies by inspector version.

## Steps

1. Follow liongard-inspectors/liongard-access-pattern: resolve the environment, `liongard_launchpoint` filtered by systemType "pfSense", verify last-run success, note dataprint age. A client may have multiple firewalls — enumerate them.
2. Pick the angle and query via `liongard_metric` or `liongard_query`. Illustrative JMESPath angles — verify field paths against the live dataprint; schemas vary by inspector version:
   - Rules: firewall rule list with action, interface, source/dest — flag any-any allows.
   - Port-forwards/NAT: inbound NAT with external port → internal host — flag exposure of RDP/3389, SMB/445.
   - Version: pfSense version — compare against current release.
   - VPN: IPsec/OpenVPN tunnel/server list with peers.
   - Admins: local admin accounts and access methods.
3. For "what changed" questions, pull `liongard_detection` / `liongard_timeline` scoped to pfSense: rule adds/edits, NAT changes, version upgrades, package changes — line them up against the incident window.
4. Sanity-check surprising values (zero rules) — usually a wrong field path or partial inspection. Re-probe broadly.
5. Output: the requested facts in a compact table, source + dataprint age line, and flags (risky inbound NAT, any-any rules, outdated version, unexpected admins). Offer a plain-text `add_ticket_note`.

## How this feeds tickets

- **Triage**: attach the rule/NAT facts to a connectivity ticket so the tech starts with the topology.
- **Exposure review**: inbound NAT to RDP/SMB and any-any rules are recurring risk findings — surface with the as-of date, recommend the tech confirm live.
- **Incident correlation**: an outage minutes after a detected rule or version change is a lead — label it time-adjacency, not proven cause.

## Guardrails

- Read-only: this skill never edits firewall config. Changes go through the partner's change process (see devices-and-infrastructure/firewall-rule-change).
- Never paste PSKs, VPN pre-shared keys, or credentials into notes even if the dataprint exposes them — reference "PSK on file in <doc platform>".
- Always state dataprint age; firewall config changes deserve a "verify live" caveat on active incidents.
- If no pfSense launchpoint exists, degrade per the access pattern: docs → ticket history → "verify on appliance".
- Exposure findings are risk inputs, not confirmed breaches — "external RDP NAT present, verify need" phrasing.
- Plain-text notes only.
