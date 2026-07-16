---
name: Liongard Zoom Read
description: Interrogate a client's Zoom account through the Liongard Zoom inspector — users/licenses, admins/roles, SSO and security settings, account-wide meeting/recording policy. Use for "who's on their Zoom?", license reconciliation, security-posture checks, or offboarding reviews during triage.
category: Liongard Inspectors
tools: [liongard_environment, liongard_launchpoint, liongard_metric, liongard_query, liongard_detection, liongard_timeline, search_tickets, search_itglue, search_hudu, add_ticket_note]
---

# Liongard Zoom Read

Reads Zoom account state — users/licenses, admins/roles, SSO and security settings, and account-wide meeting/recording policy — from the Liongard Zoom inspector's dataprint, so a tech can run access/license/security reviews without logging into the Zoom admin portal.

## When to use

- "Who has a Zoom account at <client>?" — access review or offboarding.
- "How many licensed vs basic users?" — seat reconciliation.
- "Who has admin?" / "Is SSO enforced?" — security-posture check.
- "What are the account meeting/recording security defaults?" (passcodes, waiting room, cloud-recording).
- Post-change: "did an account security setting or role change?"

## What the inspector captures

Users (status, license type), admins/roles, SSO and authentication settings, and account-wide meeting/recording security policy. Exact coverage varies by inspector version.

## Steps

1. Follow liongard-inspectors/liongard-access-pattern: resolve the environment, `liongard_launchpoint` filtered by systemType "Zoom", verify last-run success, note dataprint age.
2. Pick the angle and query via `liongard_metric` or `liongard_query`. Illustrative JMESPath angles — verify field paths against the live dataprint; schemas vary by inspector version:
   - Users: user list with status and license — count licensed vs basic, inactive-but-licensed.
   - Admins: admin/role assignments.
   - Security: SSO enforcement, meeting passcode/waiting-room defaults, cloud-recording policy.
3. For "what changed" questions, pull `liongard_detection` / `liongard_timeline` scoped to Zoom: user adds/removes, role changes, security-setting changes — line them up against the incident window.
4. Sanity-check surprising values (zero users, no admins) — usually a wrong field path or partial inspection. Re-probe broadly.
5. Output: the requested facts in a compact table, source + dataprint age line, and flags (SSO not enforced, weak meeting defaults, inactive licensed users, excess admins). Offer a plain-text `add_ticket_note`.

## How this feeds tickets

- **Offboarding**: confirm a departing user's Zoom account — feeds the offboarding checklist. Findings are inputs, not the removal action.
- **Security review**: SSO enforcement and meeting-security defaults are recurring audit findings — export with their as-of date.
- **License reconciliation**: licensed vs basic and inactive-licensed surfaces reclaimable seats.

## Guardrails

- Read-only: this skill never removes users, changes roles, or edits settings. Remediation is a tech's job in the Zoom portal. Never convert a finding into a completed action.
- Always state dataprint age; account state changes — an old dataprint on an active offboarding deserves a "verify live" caveat.
- If no Zoom launchpoint exists, degrade per the access pattern: docs → ticket history → "verify in portal".
- Access findings are review inputs, not accusations — "unrecognized admin, verify" phrasing.
- Plain-text notes only.
