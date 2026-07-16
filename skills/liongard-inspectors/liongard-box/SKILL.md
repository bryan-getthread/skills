---
name: Liongard Box Read
description: Interrogate a client's Box enterprise through the Liongard Box inspector — users/licenses, admins, sharing/security settings, groups, external-collaboration exposure. Use for "who's in their Box?", sharing/security-posture checks, or offboarding/license reviews during triage.
category: Liongard Inspectors
tools: [liongard_environment, liongard_launchpoint, liongard_metric, liongard_query, liongard_detection, liongard_timeline, search_tickets, search_itglue, search_hudu, add_ticket_note]
---

# Liongard Box Read

Reads Box enterprise state — users/licenses, admins, sharing and security settings, groups, and external-collaboration exposure — from the Liongard Box inspector's dataprint, so a tech can run access/sharing reviews without logging into the Box admin console.

## When to use

- "Who has a Box account at <client>?" — access review or offboarding.
- "How many licenses / how many active?" — seat reconciliation.
- "Who has admin / co-admin?" / "Is 2FA enforced?" — security-posture check.
- "What external-collaboration/sharing is allowed?" — data-exposure question.
- Post-change: "did a sharing/security setting change on the Box side?"

## What the inspector captures

Users (status, license type), admins and co-admins, groups, enterprise sharing and security settings (2FA enforcement, external collaboration), and exposure signals. Exact coverage varies by inspector version.

## Steps

1. Follow liongard-inspectors/liongard-access-pattern: resolve the environment, `liongard_launchpoint` filtered by systemType "Box", verify last-run success, note dataprint age.
2. Pick the angle and query via `liongard_metric` or `liongard_query`. Illustrative JMESPath angles — verify field paths against the live dataprint; schemas vary by inspector version:
   - Users: user list with status and license — count active vs total seats.
   - Admins: admin/co-admin accounts.
   - Security: 2FA enforcement flag, external-collaboration policy.
   - Groups: group membership for a named user (offboarding).
3. For "what changed" questions, pull `liongard_detection` / `liongard_timeline` scoped to Box: user adds/removes, admin changes, sharing/security-setting changes — line them up against the incident window.
4. Sanity-check surprising values (zero users, no admins) — usually a wrong field path or partial inspection. Re-probe broadly.
5. Output: the requested facts in a compact table, source + dataprint age line, and flags (2FA not enforced, permissive external collaboration, ex-employee still active, excess admins). Offer a plain-text `add_ticket_note`.

## How this feeds tickets

- **Offboarding**: confirm a departing user's account and group access — feeds the offboarding checklist. Findings are inputs, not the removal action.
- **Security review**: 2FA enforcement and external-collaboration policy are recurring audit findings — export with their as-of date.
- **License reconciliation**: active vs total seats surfaces reclaimable licenses.

## Guardrails

- Read-only: this skill never removes users, changes sharing, or edits settings. Remediation is a tech's job in the console. Never convert a finding into a completed action.
- Always state dataprint age; enterprise state changes — an old dataprint on an active offboarding deserves a "verify live" caveat.
- If no Box launchpoint exists, degrade per the access pattern: docs → ticket history → "verify in console".
- Access/sharing findings are review inputs, not accusations — "unrecognized user, verify" phrasing.
- Plain-text notes only.
