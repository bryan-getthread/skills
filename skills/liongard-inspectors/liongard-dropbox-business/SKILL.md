---
name: Liongard Dropbox Business Read
description: Interrogate a client's Dropbox Business team through the Liongard Dropbox Business inspector — members/licenses, admins, sharing/security settings, groups, external sharing exposure. Use for "who's on their Dropbox team?", sharing/security-posture checks, or offboarding/license reviews during triage.
category: Liongard Inspectors
tools: [liongard_environment, liongard_launchpoint, liongard_metric, liongard_query, liongard_detection, liongard_timeline, search_tickets, search_itglue, search_hudu, add_ticket_note]
---

# Liongard Dropbox Business Read

Reads Dropbox Business team state — members/licenses, admins, sharing and security settings, groups, and external-sharing exposure — from the Liongard Dropbox Business inspector's dataprint, so a tech can run access/sharing reviews without logging into the Dropbox admin console.

## When to use

- "Who's a member of <client>'s Dropbox Business team?" — access review or offboarding.
- "How many licenses / how many used?" — seat reconciliation.
- "Who has admin?" / "Is 2-step verification enforced?" — security-posture check.
- "What external sharing is allowed?" — data-exposure question.
- Post-change: "did a sharing/security setting change on the Dropbox side?"

## What the inspector captures

Team members (status, license), administrators, groups, team-wide sharing and security settings (2SV enforcement, external sharing), and exposure signals. Exact coverage varies by inspector version.

## Steps

1. Follow liongard-inspectors/liongard-access-pattern: resolve the environment, `liongard_launchpoint` filtered by systemType "Dropbox" / "Dropbox Business", verify last-run success, note dataprint age.
2. Pick the angle and query via `liongard_metric` or `liongard_query`. Illustrative JMESPath angles — verify field paths against the live dataprint; schemas vary by inspector version:
   - Members: member list with status and license — count used vs total seats.
   - Admins: admin accounts with role.
   - Security: 2SV enforcement flag, external-sharing policy.
   - Groups: group membership for a named user (offboarding).
3. For "what changed" questions, pull `liongard_detection` / `liongard_timeline` scoped to Dropbox: member adds/removes, admin changes, sharing/security-setting changes — line them up against the incident window.
4. Sanity-check surprising values (zero members, no admins) — usually a wrong field path or partial inspection. Re-probe broadly.
5. Output: the requested facts in a compact table, source + dataprint age line, and flags (2SV not enforced, permissive external sharing, ex-employee still a member, excess admins). Offer a plain-text `add_ticket_note`.

## How this feeds tickets

- **Offboarding**: confirm a departing user's team membership and group access — feeds the offboarding checklist. Findings are inputs, not the removal action.
- **Security review**: 2SV enforcement and external-sharing policy are recurring audit findings — export with their as-of date.
- **License reconciliation**: used vs total seats surfaces reclaimable licenses.

## Guardrails

- Read-only: this skill never removes members, changes sharing, or edits settings. Remediation is a tech's job in the console. Never convert a finding into a completed action.
- Always state dataprint age; team state changes — an old dataprint on an active offboarding deserves a "verify live" caveat.
- If no Dropbox Business launchpoint exists, degrade per the access pattern: docs → ticket history → "verify in console".
- Access/sharing findings are review inputs, not accusations — "unrecognized member, verify" phrasing.
- Plain-text notes only.
