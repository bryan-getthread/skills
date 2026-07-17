---
name: Liongard SonicWall Read
description: Interrogate a client's SonicWall through the Liongard inspector — SonicOS firmware, security-services licensing and expiry, access rules, VPN policies, admin accounts. Use for firewall triage context, the classic "security services just expired" check, or posture review.
category: Liongard Inspectors
tools: [liongard_environment, liongard_launchpoint, liongard_metric, liongard_query, liongard_detection, liongard_timeline]
connectors: [Liongard]
---

# Liongard SonicWall Read

**When to use:** "Are <client>'s SonicWall security services current?" (or a content-filtering/AV ticket that smells like an expired subscription), a SonicOS firmware-currency check, VPN policy inventory, "who can log into their SonicWall?", or "did the firewall rules change before this outage?".

## Prompt

```
Read SonicWall configuration and licensing state for CLIENT_NAME from the Liongard SonicWall inspector. Read-only — renewals, rule changes, and upgrades are human-owned.

1. Resolve the environment (liongard_environment), then liongard_launchpoint filtered by systemType "SonicWall". Verify last-run success and note the dataprint age — carry "as of <timestamp>." Multiple appliances (HA, branch units) → enumerate and answer per device.
2. Query the angle via liongard_metric or liongard_query. Verify every JMESPath field path against the live dataprint (schemas vary by inspector version):
   - Licensing (the priority read): per-service license status + expiry (Gateway AV, Anti-Spyware, IPS, Content Filtering, support) — flag anything expired or expiring within 90 days and state the impact in plain terms ("content filtering will stop enforcing"). Get expiry dates verbatim, never estimated, and carry the dataprint age (an expiry read from a week-old dataprint may already have been renewed).
   - Firmware: SonicOS version string; report as-is and recommend verifying against SonicWall's current advisories rather than asserting vulnerability from memory.
   - Rules: access-rule count, and any-any allow rules or rules with logging disabled.
   - VPN: policy list with peer gateways and enabled state.
   - Admins: accounts; flag default "admin" still enabled alongside named accounts, or management exposed on WAN zones if visible.
3. "What changed?" → liongard_detection / liongard_timeline: rule changes, admin changes, firmware upgrades, license-state transitions. A license expiring is a change detection worth catching before the tickets arrive — label incident correlations time-adjacency, not proven cause.
4. Never reproduce shared secrets or credentials from VPN policy data in notes. Output: facts + a dedicated license table (service / status / expiry), source + dataprint age, flags, and follow-ups (renewal quote, firmware review). Offer a plain-text add_ticket_note. Degradation: inspector absent → docs (search_itglue/search_hudu) → ticket history (search_tickets) → "verify on device"; a SonicWall inspector failing auth repeatedly is its own finding.
```
