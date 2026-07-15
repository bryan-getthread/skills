---
name: Liongard WatchGuard Config Read
description: Interrogate a client's WatchGuard Firebox posture through Liongard — Fireware version, subscription/licensing state, policy inventory, VPN config, admin accounts. Use for config/state questions on WatchGuard gear; for responding to live WatchGuard ALERTS use vendor-runbooks/watchguard-firewall-alerts instead.
category: Liongard Inspectors
tools: [liongard_environment, liongard_launchpoint, liongard_metric, liongard_query, liongard_detection, liongard_timeline, search_tickets, search_itglue, search_hudu, add_ticket_note]
---

# Liongard WatchGuard Config Read

Reads WatchGuard Firebox configuration posture — Fireware OS version, security-service subscriptions, policies, VPN, admin access — from the Liongard inspector. This is the **state/posture read** side; the **alert-response** side (an alarm fired, what do we do) lives in vendor-runbooks/watchguard-firewall-alerts — cross-reference it when an alert investigation needs config context, and hand off to it when the trigger is an alert rather than a question.

## When to use

- "What Fireware version is <client>'s Firebox on?" — currency/advisory check.
- "Are their WatchGuard subscriptions (WebBlocker, Gateway AV, APT Blocker, support) current?"
- VPN ticket needs the BOVPN/mobile-VPN inventory.
- "Did the Firebox config change before this started?" — pairs with the alert runbook during incidents.
- Admin-access review on the Firebox.

## What the inspector captures

Device identity and Fireware version, licensed feature/subscription state and expirations, firewall policy set, VPN configuration (branch office and mobile), and administrative accounts. Inspector availability and field coverage vary by Liongard version — confirm the launchpoint exists per the access pattern rather than assuming.

## Steps

1. Follow liongard-inspectors/liongard-access-pattern: resolve environment, `liongard_launchpoint` with systemType "WatchGuard" (try "Firebox" variant), verify last-run success, note dataprint age.
2. Query via `liongard_metric` or `liongard_query`. Illustrative JMESPath angles — verify field paths against the live dataprint; schemas vary by inspector version:
   - Fireware version — report verbatim; recommend verifying against WatchGuard's current advisories rather than asserting EOL/vulnerable from memory.
   - Subscriptions: per-service status + expiry — flag expired or <90 days, and translate to impact ("WebBlocker stops filtering").
   - Policies: count and any-any allows; disabled logging on allow policies.
   - VPN: BOVPN gateways/tunnels and mobile-VPN types enabled.
   - Admins: account list; default admin still active alongside named accounts is a flag.
3. "What changed": `liongard_detection` / `liongard_timeline` — policy, admin, firmware, and subscription-state changes ordered against the window in question.
4. If the request originated from a live WatchGuard alert, gather the config context here, then continue the response in vendor-runbooks/watchguard-firewall-alerts — attach this skill's facts (version, recent changes, subscription state) to that investigation.
5. Output: facts + subscription table, source + dataprint age, flags, and a plain-text `add_ticket_note` on request.

## How this feeds tickets

- **Triage**: "web filtering stopped" or VPN tickets open with subscription/tunnel facts attached.
- **Incident correlation**: config-change detections vs alert/outage window — time-adjacency, bounded by inspector cadence.
- **QBR / renewals**: Fireware currency and subscription expiries feed QBR posture and the renewal pipeline.

## Guardrails

- Read-only; changes, renewals, and upgrades are human-owned.
- Scope discipline: posture reads live here; alert response lives in vendor-runbooks/watchguard-firewall-alerts. Don't duplicate its escalation logic — hand off.
- Subscription expiries verbatim from the dataprint with its age attached (may have renewed since last run).
- Never reproduce VPN pre-shared keys or credential material in notes.
- Degrade per the access pattern (docs → ticket history → verify-on-device) when the inspector is absent.
- Plain-text notes only.
