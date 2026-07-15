---
name: Liongard FortiGate Read
description: Interrogate a client's FortiGate through the Liongard Fortinet inspector — firmware/FortiOS version, policy count and changes, VPN tunnel inventory, admin accounts, license/support state. Use for firewall config questions during triage, VPN incident context, or security-posture checks.
category: Liongard Inspectors
tools: [liongard_environment, liongard_launchpoint, liongard_metric, liongard_query, liongard_detection, liongard_timeline, search_tickets, search_itglue, search_hudu, add_ticket_note]
---

# Liongard FortiGate Read

Reads FortiGate configuration posture — FortiOS version, firewall policies, VPN tunnels, admin access, interface layout, licensing — from the Liongard Fortinet FortiGate inspector, so firewall questions get answered from the dataprint rather than a console session.

## When to use

- "What FortiOS version is <client> running?" — especially against a published PSIRT advisory.
- VPN ticket: "what tunnels exist and what are their peers?" before diving into logs.
- "Who has admin on their FortiGate?" / "is admin access open to the WAN?"
- "Did their firewall policies change recently?" — post-change incident or audit pass.
- License/support expiry check (FortiGuard subscriptions, support contract).

## What the inspector captures

Device identity and FortiOS firmware version, firewall policy set, interface/zone configuration, IPsec/SSL-VPN tunnel definitions, administrator accounts and trusted-host restrictions, HA state where configured, and license/subscription status. Coverage varies by inspector and FortiOS version.

## Steps

1. Follow liongard-inspectors/liongard-access-pattern: resolve environment, `liongard_launchpoint` with systemType "FortiGate" (try "Fortinet" too), verify last-run success, note dataprint age. HA pairs or multiple sites may mean multiple launchpoints — enumerate and answer per device.
2. Query the angle via `liongard_metric` or `liongard_query`. Illustrative JMESPath angles — verify field paths against the live dataprint; schemas vary by inspector version:
   - Firmware: the FortiOS version string — compare against the vendor's current advisories via `search_thread_docs`-independent judgment: report the version and let the tech map it to advisories, or state the advisory only if you can verify it.
   - Policies: policy count, and policies where action == accept with source/destination "all" — the overly-broad-rule flag.
   - VPN: tunnel list with name, remote peer, and status fields if present — the tunnel inventory for a VPN incident.
   - Admins: admin accounts with their trusted-host settings — empty trusted-host on a WAN-reachable interface is the finding.
   - Licensing: subscription/support expiry dates — flag anything within 90 days.
3. "What changed": `liongard_detection` / `liongard_timeline` for the FortiGate system — policy adds/edits, admin account changes, firmware upgrades. Policy-change detections around an outage window are the highest-value correlation.
4. Output: facts table, source + dataprint age, flags (EOL/vulnerable-looking firmware stated as "verify against Fortinet PSIRT", broad policies, unrestricted admin access, expiring subscriptions, unexplained policy changes). Offer a plain-text `add_ticket_note`.

## How this feeds tickets

- **Triage**: VPN and "site can't reach X" tickets open with tunnel/policy facts attached.
- **Incident correlation**: detected policy or firmware changes lined up against the outage window — time-adjacency, not proven cause.
- **QBR / security posture**: firmware currency, subscription expiries, and admin-hygiene findings are QBR and security-review inputs (cross-ref security category skills for the response side).

## Guardrails

- Read-only; policy or firmware changes are human-owned and go through change control (devices-and-infrastructure/firewall-rule-change).
- Do not label a firmware version "vulnerable" from memory — report the version and recommend verification against Fortinet's current PSIRT advisories.
- Never reproduce PSKs, certificates, or credential material from the dataprint in notes.
- A dataprint hours old can miss the change that caused the current incident — state age and say "config may have changed since last inspection" on active incidents.
- Degrade to docs/ticket-history per the access pattern when the inspector is absent or failing; repeated inspector auth failures on a firewall are themselves worth a ticket (credentials rotated?).
- Plain-text notes only.
