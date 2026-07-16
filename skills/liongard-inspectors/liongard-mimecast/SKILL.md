---
name: Liongard Mimecast Read
description: Interrogate a client's Mimecast tenant through the Liongard Mimecast inspector — managed domains, users/licenses, policies, connectors/routing, admins. Use for "what's their Mimecast config?", mail-security posture checks, or domain-coverage questions during triage.
category: Liongard Inspectors
tools: [liongard_environment, liongard_launchpoint, liongard_metric, liongard_query, liongard_detection, liongard_timeline, search_tickets, search_itglue, search_hudu, add_ticket_note]
---

# Liongard Mimecast Read

Reads Mimecast tenant state — managed domains, users/licenses, gateway policies, connectors/routing, and admins — from the Liongard Mimecast inspector's dataprint, so a tech can judge email-security posture without logging into the Mimecast console.

## When to use

- "What domains does Mimecast manage for <client>?" — coverage check.
- Mail-delivery or phishing ticket needs the policy/routing picture before triage.
- "Which users are licensed / how many seats?" — license reconciliation.
- "What gateway/connector routing is configured?"
- Post-change: "did a policy or connector change on the Mimecast side?"

## What the inspector captures

Managed domains (with verification/routing state), licensed users/seats, gateway and anti-spam/impersonation policies, connectors/routing, and tenant admins. Exact coverage varies by inspector version.

## Steps

1. Follow liongard-inspectors/liongard-access-pattern: resolve the environment, `liongard_launchpoint` filtered by systemType "Mimecast", verify last-run success, note dataprint age.
2. Pick the angle and query via `liongard_metric` or `liongard_query`. Illustrative JMESPath angles — verify field paths against the live dataprint; schemas vary by inspector version:
   - Domains: managed-domain list with verification/routing state.
   - Users: licensed users and seat count vs mailbox count.
   - Policy: gateway/impersonation/anti-spam policy summary.
   - Connectors: inbound/outbound routing config.
   - Admins: admin list with role.
3. For "what changed" questions, pull `liongard_detection` / `liongard_timeline` scoped to Mimecast: domain adds/removes, policy/connector changes, admin changes — line them up against the incident window.
4. Sanity-check surprising values (zero domains) — usually a wrong field path or partial inspection. Re-probe broadly.
5. Output: the requested facts in a compact table, source + dataprint age line, and flags (unverified domains, seat/mailbox mismatch, unexpected connector routing). Offer a plain-text `add_ticket_note`.

## How this feeds tickets

- **Triage**: attach the domain/policy/routing picture to a mail-delivery or phishing ticket.
- **License reconciliation**: Mimecast seat count vs M365/mailbox count surfaces over/under-licensing — label the delta.
- **QBR evidence**: managed-domain coverage and policy posture are recurring QBR line items — export with their as-of date.

## Guardrails

- Read-only: this skill never changes Mimecast policy, domains, connectors, or licensing. Remediation is a tech's job in the console.
- Always state dataprint age; mail-security config changes — an old dataprint on an active delivery incident deserves a "verify live" caveat.
- If no Mimecast launchpoint exists, degrade per the access pattern: docs → ticket history → "verify in console".
- Admin-list findings are access-review inputs, not accusations — "unrecognized admin, verify" phrasing.
- Plain-text notes only.
