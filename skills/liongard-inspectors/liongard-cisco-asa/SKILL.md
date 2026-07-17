---
name: Liongard Cisco ASA Read
description: Interrogate a client's Cisco ASA firewall through the Liongard Cisco ASA inspector — software version, interfaces, access lists/NAT, VPN (IPsec/AnyConnect) config, admin access. Use for "what's their ASA config?", ACL/exposure review during triage, or version-currency checks.
category: Liongard Inspectors
tools: [liongard_environment, liongard_launchpoint, liongard_metric, liongard_query, liongard_detection, liongard_timeline]
connectors: [Liongard]
---

# Liongard Cisco ASA Read

**When to use:** "What access lists / NAT rules are configured at <client>?", a connectivity or exposure ticket needing the ACL/NAT picture, "what ASA version are they on?", a VPN (IPsec site-to-site / AnyConnect) troubleshooting read, or "did an ACL or NAT entry change?".

## Prompt

```
Read Cisco ASA firewall config for CLIENT_NAME from the Liongard Cisco ASA inspector. Read-only — ASA changes go through the partner's change process.

1. Resolve the environment (liongard_environment), then liongard_launchpoint filtered by systemType "Cisco ASA" / "ASA". Verify last-run success and note the dataprint age — carry "as of <timestamp>." A client may have multiple ASAs — enumerate them.
2. Pick the angle and query via liongard_metric or liongard_query. Verify every JMESPath field path against the live dataprint (schemas vary by inspector version):
   - ACLs: access-list entries with action, source/dest, service — flag permit any any.
   - NAT: static/inbound NAT with external port to internal host — flag exposure of RDP/3389, SMB/445.
   - Version: ASA software version — report verbatim and recommend verifying against current advisories, not memory.
   - VPN: IPsec site-to-site peers and AnyConnect config.
   - Admins: local admin accounts and access methods (SSH/ASDM).
3. "What changed?" → liongard_detection / liongard_timeline scoped to the ASA: ACL adds/edits, NAT changes, version upgrades, admin changes — ordered against the incident window (time-adjacency, not proven cause).
4. Sanity-check surprising zeros (zero ACL entries) — usually a wrong field path or partial inspection; re-probe broadly. Never paste PSKs, VPN pre-shared keys, or credentials into notes — reference "PSK on file in <doc platform>." Exposure findings are risk inputs, not confirmed breaches — "external RDP NAT present, verify need" phrasing.
5. Output: a compact table, source + dataprint age line, flags (permit-any ACLs, risky inbound NAT, outdated version, unexpected admins). Offer a plain-text add_ticket_note. Degradation: no ASA launchpoint → docs (search_itglue/search_hudu) → ticket history (search_tickets) → "verify on appliance"; also check the generic Cisco network inspector.
```
