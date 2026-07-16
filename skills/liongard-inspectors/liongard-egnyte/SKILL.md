---
name: Liongard Egnyte Read
description: Interrogate a client's Egnyte domain through the Liongard Egnyte inspector — users/licenses, admins, groups, sharing/security settings, external-sharing exposure. Use for "who's in their Egnyte?", sharing/security-posture checks, or offboarding/license reviews during triage.
category: Liongard Inspectors
tools: [liongard_environment, liongard_launchpoint, liongard_metric, liongard_query, liongard_detection, liongard_timeline, search_tickets, search_itglue, search_hudu, add_ticket_note]
---

# Liongard Egnyte Read

Reads Egnyte domain state — users/licenses, admins, groups, sharing and security settings, and external-sharing exposure — from the Liongard Egnyte inspector's dataprint, so a tech can run access/sharing reviews without logging into the Egnyte admin console.

## When to use

- "Who has an Egnyte account at <client>?" — access review or offboarding.
- "How many licenses / how many active?" — seat reconciliation.
- "Who has admin?" / "Is 2-step login enforced?" — security-posture check.
- "What external sharing is allowed?" — data-exposure question.
- Post-change: "did a sharing/security setting change on the Egnyte side?"

## What the inspector captures

Users (status, role, license), administrators, groups, domain sharing and security settings (2-step verification, external sharing/link policy), and exposure signals. Exact coverage varies by inspector version.

## Steps

1. Follow liongard-inspectors/liongard-access-pattern: resolve the environment, `liongard_launchpoint` filtered by systemType "Egnyte", verify last-run success, note dataprint age.
2. Pick the angle and query via `liongard_metric` or `liongard_query`. Illustrative JMESPath angles — verify field paths against the live dataprint; schemas vary by inspector version:
   - Users: user list with role/status and license — count active vs total seats.
   - Admins: admin accounts.
   - Security: 2-step verification flag, external link/sharing policy.
   - Groups: group membership for a named user (offboarding).
3. For "what changed" questions, pull `liongard_detection` / `liongard_timeline` scoped to Egnyte: user adds/removes, admin changes, sharing/security-setting changes — line them up against the incident window.
4. Sanity-check surprising values (zero users, no admins) — usually a wrong field path or partial inspection. Re-probe broadly.
5. Output: the requested facts in a compact table, source + dataprint age line, and flags (2-step not enforced, permissive external sharing, ex-employee still active, excess admins). Offer a plain-text `add_ticket_note`.

## How this feeds tickets

- **Offboarding**: confirm a departing user's account and group access — feeds the offboarding checklist. Findings are inputs, not the removal action.
- **Security review**: 2-step enforcement and external-sharing policy are recurring audit findings — export with their as-of date.
- **License reconciliation**: active vs total seats surfaces reclaimable licenses.

## Guardrails

- Read-only: this skill never removes users, changes sharing, or edits settings. Remediation is a tech's job in the console. Never convert a finding into a completed action.
- Always state dataprint age; domain state changes — an old dataprint on an active offboarding deserves a "verify live" caveat.
- If no Egnyte launchpoint exists, degrade per the access pattern: docs → ticket history → "verify in console".
- Access/sharing findings are review inputs, not accusations — "unrecognized user, verify" phrasing.
- Plain-text notes only.
