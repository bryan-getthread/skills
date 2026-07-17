---
name: Liongard Sophos Firewall Read
description: Interrogate a client's Sophos Firewall (XG/SFOS) through the Liongard Sophos Firewall inspector — firmware, firewall rules, port-forwards/NAT, VPN config, interfaces, admin access. Use for "what's their Sophos firewall config?", rule/exposure review during triage, or firmware-currency checks.
category: Liongard Inspectors
tools: [liongard_environment, liongard_launchpoint, liongard_metric, liongard_query, liongard_detection, liongard_timeline]
connectors: [Liongard]
---

# Liongard Sophos Firewall Read

**When to use:** "What firewall rules / port-forwards are configured at <client>?", a connectivity or exposure ticket, "what firmware is their Sophos firewall on?", a VPN troubleshooting read, or "did a rule or NAT entry change?".

## Prompt

```
Read Sophos Firewall (SFOS / XG) config for CLIENT_NAME from the Liongard Sophos Firewall inspector. Read-only — firewall changes go through the partner's change process.

1. Resolve the environment (liongard_environment), then liongard_launchpoint filtered by systemType "Sophos Firewall" / "SFOS" / "XG". Verify last-run success and note the dataprint age — carry "as of <timestamp>." A client may have multiple firewalls — enumerate them.
2. Pick the angle and query via liongard_metric or liongard_query. Verify every JMESPath field path against the live dataprint (schemas vary by inspector version):
   - Rules: firewall rule list with action, source/dest zones, service — flag any-any allows.
   - Port-forwards/NAT: inbound NAT entries with external port to internal host — flag exposure of RDP/3389, SMB/445.
   - Firmware: current SFOS version — report verbatim, compare against vendor's current release.
   - VPN: IPsec/SSL tunnel list with peers and status.
   - Admins: local/remote admin accounts and access methods.
3. "What changed?" → liongard_detection / liongard_timeline scoped to the firewall: rule adds/edits, NAT changes, firmware upgrades, admin adds — ordered against the incident window (time-adjacency, not proven cause).
4. Sanity-check surprising zeros (zero rules) — usually a wrong field path or partial inspection; re-probe broadly. Never paste PSKs, VPN pre-shared keys, or credentials into notes — reference "PSK on file in <doc platform>." Exposure findings are risk inputs, not confirmed breaches — "external RDP NAT present, verify need" phrasing.
5. Output: a compact table, source + dataprint age line, flags (risky inbound NAT, any-any rules, outdated firmware, unexpected admins). Offer a plain-text add_ticket_note. Degradation: no Sophos Firewall launchpoint → docs (search_itglue/search_hudu) → ticket history (search_tickets) → "verify on appliance."
```
