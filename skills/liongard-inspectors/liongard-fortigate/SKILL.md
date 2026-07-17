---
name: Liongard FortiGate Read
description: Interrogate a client's FortiGate through the Liongard Fortinet inspector — firmware/FortiOS version, policy count and changes, VPN tunnel inventory, admin accounts, license/support state. Use for firewall config questions during triage, VPN incident context, or security-posture checks.
category: Liongard Inspectors
tools: [liongard_environment, liongard_launchpoint, liongard_metric, liongard_query, liongard_detection, liongard_timeline]
connectors: [Liongard]
---

# Liongard FortiGate Read

**When to use:** "What FortiOS version is <client> running?" against a PSIRT advisory, VPN tunnel/peer inventory before diving into logs, "who has admin / is admin access open to the WAN?", "did their firewall policies change?", or a FortiGuard/support expiry check.

## Prompt

```
Read FortiGate configuration posture for CLIENT_NAME from the Liongard Fortinet FortiGate inspector. Read-only — policy or firmware changes are human-owned through change control.

1. Resolve the environment (liongard_environment), then liongard_launchpoint filtered by systemType "FortiGate" (try "Fortinet" too). Verify last-run success and note the dataprint age — carry "as of <timestamp>" on every answer. HA pairs or multiple sites mean multiple launchpoints — enumerate and answer per device.
2. Query the angle via liongard_metric or liongard_query. Verify every JMESPath field path against the live dataprint (schemas vary by inspector version):
   - Firmware: the FortiOS version string — report it verbatim and let the tech map it to advisories; do NOT label a version "vulnerable" from memory — recommend verifying against Fortinet's current PSIRT advisories.
   - Policies: policy count, and policies where action == accept with source/destination "all" — the overly-broad-rule flag.
   - VPN: tunnel list with name, remote peer, status — the inventory for a VPN incident.
   - Admins: admin accounts with trusted-host settings — empty trusted-host on a WAN-reachable interface is the finding.
   - Licensing: subscription/support expiry — flag anything within 90 days.
3. "What changed?" → liongard_detection / liongard_timeline scoped to the FortiGate: policy adds/edits, admin changes, firmware upgrades. Policy-change detections around an outage window are the highest-value correlation — label it time-adjacency, not proven cause. A dataprint hours old can miss the change that caused the incident; state age and say "config may have changed since last inspection" on active incidents.
4. Never reproduce PSKs, certificates, or credential material from the dataprint in notes. Output: facts table, source + dataprint age, flags (firmware stated as "verify against Fortinet PSIRT", broad policies, unrestricted admin access, expiring subscriptions, unexplained policy changes). Offer a plain-text add_ticket_note. Degradation: inspector absent/failing → docs (search_itglue/search_hudu) → ticket history (search_tickets) → "verify on device"; repeated inspector auth failures on a firewall are themselves worth a ticket.
```
