---
name: Liongard Salesforce Read
description: Interrogate a client's Salesforce org through the Liongard Salesforce inspector — users/licenses, admins/profiles, MFA and login/security settings, connected apps. Use for "who's in their Salesforce?", license reconciliation, admin/security reviews, or offboarding checks during triage.
category: Liongard Inspectors
tools: [liongard_environment, liongard_launchpoint, liongard_metric, liongard_query, liongard_detection, liongard_timeline, search_tickets, search_itglue, search_hudu, add_ticket_note]
---

# Liongard Salesforce Read

Reads Salesforce org state — users/licenses, admins and profiles, MFA and login/security settings, and connected apps — from the Liongard Salesforce inspector's dataprint, so a tech can run access/license reviews without logging into Salesforce Setup.

## When to use

- "Who has a Salesforce login at <client>?" — access review or offboarding.
- "How many licenses / how many active users?" — seat reconciliation.
- "Who has the System Administrator profile?" — admin review.
- "Is MFA enforced?" / "What login IP/security settings are set?" — security-posture check.
- Post-change: "did a profile, connected app, or security setting change?"

## What the inspector captures

Users (status, profile, license type), administrator/profile assignments, MFA and login/session security settings, and connected/OAuth apps. Exact coverage varies by inspector version.

## Steps

1. Follow liongard-inspectors/liongard-access-pattern: resolve the environment, `liongard_launchpoint` filtered by systemType "Salesforce", verify last-run success, note dataprint age.
2. Pick the angle and query via `liongard_metric` or `liongard_query`. Illustrative JMESPath angles — verify field paths against the live dataprint; schemas vary by inspector version:
   - Users: user list with status/profile/license — count active vs total, inactive-but-licensed.
   - Admins: users with the System Administrator profile.
   - Security: MFA enforcement flag, session/login-IP settings.
   - Connected apps: OAuth/connected-app list.
3. For "what changed" questions, pull `liongard_detection` / `liongard_timeline` scoped to Salesforce: user/profile changes, security-setting changes, connected-app adds — line them up against the incident window.
4. Sanity-check surprising values (zero users, no admins) — usually a wrong field path or partial inspection. Re-probe broadly.
5. Output: the requested facts in a compact table, source + dataprint age line, and flags (MFA not enforced, excess admins, inactive licensed users, unexpected connected apps). Offer a plain-text `add_ticket_note`.

## How this feeds tickets

- **Offboarding**: confirm a departing user's login and profile — feeds the offboarding checklist. Findings are inputs, not the deactivation action.
- **Security review**: MFA enforcement, admin count, and connected apps are recurring audit findings — export with their as-of date.
- **License reconciliation**: active vs total seats surfaces reclaimable licenses.

## Guardrails

- Read-only: this skill never deactivates users, changes profiles, or edits security settings. Remediation is a tech's job in Salesforce Setup. Never convert a finding into a completed action.
- Always state dataprint age; org state changes — an old dataprint on an active offboarding deserves a "verify live" caveat.
- If no Salesforce launchpoint exists, degrade per the access pattern: docs → ticket history → "verify in Setup".
- Access/admin findings are review inputs, not accusations — "unrecognized admin, verify" phrasing.
- Plain-text notes only.
