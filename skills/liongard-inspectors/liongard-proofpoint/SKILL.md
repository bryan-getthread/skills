---
name: Liongard Proofpoint Read
description: Interrogate a client's Proofpoint Essentials tenant through the Liongard Proofpoint inspector — protected domains, users/licenses, filtering policy, spooling, admins. Use for "what's their Proofpoint config?", mail-security posture checks, or domain-coverage questions during triage.
category: Liongard Inspectors
tools: [liongard_environment, liongard_launchpoint, liongard_metric, liongard_query, liongard_detection, liongard_timeline, search_tickets, search_itglue, search_hudu, add_ticket_note]
---

# Liongard Proofpoint Read

Reads Proofpoint Essentials tenant state — protected domains, users/licenses, filtering policy, spooling/continuity, and admins — from the Liongard Proofpoint inspector's dataprint, so a tech can judge email-security posture without logging into the Proofpoint console.

## When to use

- "What domains does Proofpoint protect for <client>?" — coverage check.
- Mail-delivery or phishing ticket needs the filtering-policy picture before triage.
- "Which users are licensed / how many seats?" — license reconciliation.
- "Is email spooling/continuity enabled?"
- Post-change: "did a filtering policy or domain change on the Proofpoint side?"

## What the inspector captures

Protected domains (with routing/verification state), licensed users/seats, filtering and quarantine policy, spooling/continuity settings, and tenant admins. Exact coverage varies by inspector version.

## Steps

1. Follow liongard-inspectors/liongard-access-pattern: resolve the environment, `liongard_launchpoint` filtered by systemType "Proofpoint" / "Proofpoint Essentials", verify last-run success, note dataprint age.
2. Pick the angle and query via `liongard_metric` or `liongard_query`. Illustrative JMESPath angles — verify field paths against the live dataprint; schemas vary by inspector version:
   - Domains: protected-domain list with verification/routing state.
   - Users: licensed users and seat count vs mailbox count.
   - Policy: filtering/quarantine policy summary.
   - Continuity: spooling/continuity enabled flag.
   - Admins: admin list with role.
3. For "what changed" questions, pull `liongard_detection` / `liongard_timeline` scoped to Proofpoint: domain adds/removes, policy changes, admin changes — line them up against the incident window.
4. Sanity-check surprising values (zero domains) — usually a wrong field path or partial inspection. Re-probe broadly.
5. Output: the requested facts in a compact table, source + dataprint age line, and flags (unverified domains, seat/mailbox mismatch, continuity disabled). Offer a plain-text `add_ticket_note`.

## How this feeds tickets

- **Triage**: attach the domain/policy picture to a mail-delivery or phishing ticket so the tech starts with the filtering topology.
- **License reconciliation**: Proofpoint seat count vs M365/mailbox count surfaces over/under-licensing — label the delta.
- **QBR evidence**: protected-domain coverage and continuity posture are recurring QBR line items — export with their as-of date.

## Guardrails

- Read-only: this skill never changes Proofpoint policy, domains, or licensing. Remediation is a tech's job in the console.
- Always state dataprint age; mail-security config changes — an old dataprint on an active delivery incident deserves a "verify live" caveat.
- If no Proofpoint launchpoint exists, degrade per the access pattern: docs → ticket history → "verify in console".
- Admin-list findings are access-review inputs, not accusations — "unrecognized admin, verify" phrasing.
- Plain-text notes only.
