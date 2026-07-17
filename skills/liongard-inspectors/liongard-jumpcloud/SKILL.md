---
name: Liongard JumpCloud Read
description: Interrogate a client's JumpCloud directory through the Liongard JumpCloud inspector — users, MFA enrollment, admins, groups, bound devices/systems, SSO app assignments. Use for "who's in their JumpCloud?", MFA-coverage or admin reviews, or offboarding checks during triage.
category: Liongard Inspectors
tools: [liongard_environment, liongard_launchpoint, liongard_metric, liongard_query, liongard_detection, liongard_timeline]
connectors: [Liongard]
---

# Liongard JumpCloud Read

**When to use:** "Who has an account in <client>'s JumpCloud?" for an access review or offboarding, "which users don't have MFA enrolled?", "who has admin?", or "what devices/systems are bound?".

## Prompt

```
Answer a JumpCloud directory question for CLIENT_NAME from the Liongard JumpCloud inspector's dataprint. Read-only — disabling users, changing MFA, or editing groups is a technician's job; never convert a finding into a completed action.

1. Resolve the environment (liongard_environment), then liongard_launchpoint filtered by systemType "JumpCloud". Verify last-run success and note the dataprint age — every answer carries "as of <timestamp>."
2. Pick the angle and query via liongard_metric or liongard_query. Verify every field path against the live dataprint (schemas vary by inspector version):
   - Users: user list with status and MFA flag — count users without MFA.
   - Admins: admin accounts with role — count and name for review.
   - Groups: group membership for a named user (offboarding).
   - Devices: bound systems with last-seen.
3. "What changed?" → liongard_detection / liongard_timeline scoped to JumpCloud: user adds/removes, admin/group changes, MFA-state shifts — ordered against the incident window.
4. Sanity-check surprising zeros (no users, no admins) — usually a wrong field path or partial inspection; re-probe broadly. Absence of a user in JumpCloud is not proof they have no access elsewhere — cross-check other IdPs (Okta/Entra/Duo) before asserting.
5. Access findings are review inputs, not accusations — "unrecognized admin, verify" phrasing. Output: a compact table, source + dataprint age line, and flags (users without MFA, excess admins, stale/enabled ex-employee accounts). Offer a plain-text add_ticket_note. If no JumpCloud launchpoint exists, degrade: docs (search_itglue/search_hudu) → ticket history (search_tickets) → "verify in console."
```
