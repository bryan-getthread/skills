---
name: Liongard Cisco ASA Read
description: Interrogate a client's Cisco ASA firewall through the Liongard Cisco ASA inspector — software version, interfaces, access lists/NAT, VPN (IPsec/AnyConnect) config, admin access. Use for "what's their ASA config?", ACL/exposure review during triage, or version-currency checks.
category: Liongard Inspectors
tools: [liongard_environment, liongard_launchpoint, liongard_metric, liongard_query, liongard_detection, liongard_timeline, search_tickets, search_itglue, search_hudu, add_ticket_note]
---

# Liongard Cisco ASA Read

Reads Cisco ASA firewall config — software/ASA version, interfaces, access lists and NAT, VPN (IPsec/AnyConnect) config, and admin access — from the Liongard Cisco ASA inspector's dataprint, so a tech gets the edge picture during triage without an SSH session to the appliance.

## When to use

- "What access lists / NAT rules are configured at <client>?"
- Connectivity or exposure ticket needs the ACL and NAT picture before anyone touches the ASA.
- "What ASA/ASDM version are they on?" — currency/vulnerability check.
- "What VPN (IPsec site-to-site / AnyConnect) is configured?" — remote-access troubleshooting.
- Post-change: "did an ACL or NAT entry change?"

## What the inspector captures

ASA software version, interface/nameif layout and security levels, access lists, NAT rules, VPN (IPsec/AnyConnect) config, and administrative access. Exact coverage varies by inspector version.

## Steps

1. Follow liongard-inspectors/liongard-access-pattern: resolve the environment, `liongard_launchpoint` filtered by systemType "Cisco ASA" / "ASA", verify last-run success, note dataprint age. A client may have multiple ASAs — enumerate them.
2. Pick the angle and query via `liongard_metric` or `liongard_query`. Illustrative JMESPath angles — verify field paths against the live dataprint; schemas vary by inspector version:
   - ACLs: access-list entries with action, source/dest, service — flag permit any any.
   - NAT: static/inbound NAT with external port → internal host — flag exposure of RDP/3389, SMB/445.
   - Version: ASA software version — compare against current release / known-vuln advisories.
   - VPN: IPsec site-to-site peers and AnyConnect config.
   - Admins: local admin accounts and access methods (SSH/ASDM).
3. For "what changed" questions, pull `liongard_detection` / `liongard_timeline` scoped to the ASA: ACL adds/edits, NAT changes, version upgrades, admin changes — line them up against the incident window.
4. Sanity-check surprising values (zero ACL entries) — usually a wrong field path or partial inspection. Re-probe broadly.
5. Output: the requested facts in a compact table, source + dataprint age line, and flags (permit-any ACLs, risky inbound NAT, outdated version, unexpected admins). Offer a plain-text `add_ticket_note`.

## How this feeds tickets

- **Triage**: attach the ACL/NAT facts to a connectivity ticket so the tech starts with the topology.
- **Exposure review**: inbound NAT to RDP/SMB and permit-any ACLs are recurring risk findings — surface with the as-of date, recommend the tech confirm live.
- **Incident correlation**: an outage minutes after a detected ACL or version change is a lead — label it time-adjacency, not proven cause.

## Guardrails

- Read-only: this skill never edits ASA config. Changes go through the partner's change process (see devices-and-infrastructure/firewall-rule-change).
- Never paste PSKs, VPN pre-shared keys, or credentials into notes even if the dataprint exposes them — reference "PSK on file in <doc platform>".
- Always state dataprint age; firewall config changes deserve a "verify live" caveat on active incidents.
- If no Cisco ASA launchpoint exists, degrade per the access pattern: docs → ticket history → "verify on appliance". Also check the generic Cisco network inspector (liongard-cisco-network).
- Exposure findings are risk inputs, not confirmed breaches — "external RDP NAT present, verify need" phrasing.
- Plain-text notes only.
