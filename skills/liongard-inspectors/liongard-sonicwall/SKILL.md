---
name: Liongard SonicWall Read
description: Interrogate a client's SonicWall through the Liongard inspector — SonicOS firmware, security-services licensing and expiry, access rules, VPN policies, admin accounts. Use for firewall triage context, the classic "security services just expired" check, or posture review.
category: Liongard Inspectors
tools: [liongard_environment, liongard_launchpoint, liongard_metric, liongard_query, liongard_detection, liongard_timeline, search_tickets, search_itglue, search_hudu, add_ticket_note]
---

# Liongard SonicWall Read

Reads SonicWall configuration and licensing state — SonicOS version, security-service subscriptions, access rules, VPN policies, admin accounts — from the Liongard SonicWall inspector. The licensing angle earns its keep: expired Gateway AV / IPS / Content Filtering subscriptions silently degrade protection and cause "web filtering stopped working" tickets.

## When to use

- "Are <client>'s SonicWall security services current?" — or a content-filtering/AV ticket that smells like an expired subscription.
- "What SonicOS version are they on?" — firmware-currency or advisory check.
- VPN ticket: site-to-site and SSL-VPN policy inventory.
- "Who can log into their SonicWall?" — admin review.
- "Did the firewall rules change before this outage?"

## What the inspector captures

Device model and SonicOS firmware, security-services license state and expiration per service (Gateway AV, Anti-Spyware, IPS, Content Filtering, support contract), access rules, VPN policies (site-to-site and SSL-VPN), interface/zone layout, and administrator accounts. Coverage varies by inspector and SonicOS version.

## Steps

1. Follow liongard-inspectors/liongard-access-pattern: resolve environment, `liongard_launchpoint` with systemType "SonicWall", verify last-run success, note dataprint age. Multiple appliances (HA, branch units) → enumerate and answer per device.
2. Query the angle via `liongard_metric` or `liongard_query`. Illustrative JMESPath angles — verify field paths against the live dataprint; schemas vary by inspector version:
   - **Licensing (the priority read)**: per-service license status + expiry — flag anything expired or expiring within 90 days, and state which protection is affected in plain terms ("content filtering will stop enforcing").
   - Firmware: SonicOS version string; report as-is and recommend verifying against SonicWall's current advisories rather than asserting vulnerability from memory.
   - Rules: access-rule count, and any-any allow rules or rules with logging disabled — the broad-rule flags.
   - VPN: policy list with peer gateways and enabled state — the tunnel inventory.
   - Admins: admin accounts; flag default "admin" still enabled alongside named accounts, or management exposed on WAN zones if visible in the dataprint.
3. "What changed": `liongard_detection` / `liongard_timeline` — rule changes, admin changes, firmware upgrades, license-state transitions. A license expiring is a change detection worth catching *before* the tickets arrive.
4. Output: facts + a dedicated license table (service / status / expiry), source + dataprint age, flags, recommended follow-ups (renewal quote, firmware review). Offer a plain-text `add_ticket_note`.

## How this feeds tickets

- **Triage**: "websites aren't being blocked anymore" / "AV alerts stopped" tickets get the license table attached — often the whole answer.
- **Incident correlation**: rule or firmware changes lined up against the outage window (time-adjacency, not proven cause).
- **QBR / renewal pipeline**: expiring security services are a renewal-revenue signal as well as a protection gap — flag both framings for the account manager (cross-ref account-management skills).

## Guardrails

- Read-only; renewals, rule changes, and upgrades are human-owned.
- License findings drive quotes and renewals — get the expiry dates from the dataprint verbatim, never estimated, and carry the dataprint age (an expiry read from a week-old dataprint may already have been renewed).
- Never reproduce shared secrets or credentials from VPN policy data in notes.
- Degrade per the access pattern (docs → ticket history → "verify on device") when the inspector is absent; a SonicWall inspector failing auth repeatedly is its own finding.
- Plain-text notes only.
