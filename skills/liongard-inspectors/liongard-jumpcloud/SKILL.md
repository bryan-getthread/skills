---
name: Liongard JumpCloud Read
description: Interrogate a client's JumpCloud directory through the Liongard JumpCloud inspector — users, MFA enrollment, admins, groups, bound devices/systems, SSO app assignments. Use for "who's in their JumpCloud?", MFA-coverage or admin reviews, or offboarding checks during triage.
category: Liongard Inspectors
tools: [liongard_environment, liongard_launchpoint, liongard_metric, liongard_query, liongard_detection, liongard_timeline, search_tickets, search_itglue, search_hudu, add_ticket_note]
---

# Liongard JumpCloud Read

Reads JumpCloud directory state — users, MFA enrollment, admins, groups, bound systems/devices, and SSO app assignments — from the Liongard JumpCloud inspector's dataprint, so a tech can run access reviews without logging into the JumpCloud console.

## When to use

- "Who has an account in <client>'s JumpCloud?" — access review or offboarding check.
- "Which users don't have MFA enrolled?" — security-posture question.
- "Who has admin on their JumpCloud?"
- "What devices/systems are bound?" — coverage check.
- Post-change: "did a group membership or admin assignment change?"

## What the inspector captures

User directory (status, MFA enrollment), administrator accounts, user/system groups, bound systems/devices, and SSO application assignments. Exact coverage varies by inspector version.

## Steps

1. Follow liongard-inspectors/liongard-access-pattern: resolve the environment, `liongard_launchpoint` filtered by systemType "JumpCloud", verify last-run success, note dataprint age.
2. Pick the angle and query via `liongard_metric` or `liongard_query`. Illustrative JMESPath angles — verify field paths against the live dataprint; schemas vary by inspector version:
   - Users: user list with status and MFA flag — count users without MFA.
   - Admins: admin accounts with role — count and name for review.
   - Groups: group membership for a named user (offboarding).
   - Devices: bound systems with last-seen.
3. For "what changed" questions, pull `liongard_detection` / `liongard_timeline` scoped to JumpCloud: user adds/removes, admin/group changes, MFA-state shifts — line them up against the incident window.
4. Sanity-check surprising values (zero users, no admins) — usually a wrong field path or partial inspection. Re-probe broadly.
5. Output: the requested facts in a compact table, source + dataprint age line, and flags (users without MFA, excess admins, stale/enabled ex-employee accounts). Offer a plain-text `add_ticket_note`.

## How this feeds tickets

- **Offboarding**: confirm a departing user's account and group memberships — feeds the offboarding checklist. Findings are inputs, not the disable action itself.
- **Security review**: MFA-coverage gaps and admin counts are recurring audit findings — export with their as-of date.
- **Access review**: list who has access to what before an access-change ticket.

## Guardrails

- Read-only: this skill never disables users, changes MFA, or edits groups. Remediation is a tech's job in the console. Never convert a finding into a completed action.
- Absence of a user in JumpCloud is not proof they have no access elsewhere — cross-check other IdPs (Okta/Entra/Duo) before asserting.
- Always state dataprint age; directory state changes — an old dataprint on an active offboarding deserves a "verify live" caveat.
- If no JumpCloud launchpoint exists, degrade per the access pattern: docs → ticket history → "verify in console".
- Access findings are review inputs, not accusations — "unrecognized admin, verify" phrasing.
- Plain-text notes only.
