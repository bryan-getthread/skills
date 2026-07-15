---
name: Conditional Access Review
description: Periodic inventory and gap review of a tenant's Conditional Access policies — overlaps, legacy-auth gaps, unprotected apps, and report-only discipline for any change. Use for "review <client>'s CA policies", posture assessments, or after an incident that a CA policy should have stopped.
category: M365 Administration
tools: [search_tickets, search_knowledge_base, search_itglue, search_hudu, add_ticket_note, update_ticket, create_ticket, schedule_ticket, send_approval, web_search]
---

# Conditional Access Review

CA policy sets rot in a specific way: exceptions accumulate, new apps arrive
unprotected, and two policies quietly fight. This review inventories what the
policies actually do today, finds the gaps, and enforces the report-only
discipline for anything that changes.

## When to use

- Scheduled/periodic CA review for a managed tenant (quarterly is typical).
- "Are <client>'s conditional access policies any good?" / insurance or audit prep.
- After an identity incident, to find why CA didn't stop it.
- Before consolidating or migrating policies (e.g., off security defaults —
  see the security-defaults-vs-ca skill).

## Steps

1. **Inventory.** Tech exports every CA policy (including disabled and
   report-only). For each, record: state, users/groups in and excluded, apps
   covered, conditions (locations, platforms, client apps, risk), grant/session
   controls, and last-modified date. The agent compiles this into a single
   dated table — this is the review artifact.
2. **Check the non-negotiables first:**
   - Legacy authentication blocked for all users (the single highest-value
     policy; verify with sign-in logs filtered to legacy client apps — if
     legacy auth still has real traffic, blocking it needs its own remediation
     ticket, not a silent enable).
   - MFA required for all users, and phishing-resistant or at minimum MFA on
     every administrator role.
   - Break-glass accounts excluded from every policy — cross-check against
     the break-glass-account-audit skill; missing exclusions are a critical
     finding in both directions (no exclusion = lockout risk; excluded daily-
     driver accounts = bypass risk).
3. **Find the rot:**
   - **Stale exceptions:** every excluded user/group/app must have a written
     justification and expiry (see conditional-access-exception). Exclusions
     nobody can explain are findings.
   - **Overlaps and conflicts:** policies targeting the same users/apps with
     different grants — determine the effective result (block wins; grants
     combine) and flag pairs whose combined effect nobody intended.
   - **Coverage gaps:** apps or user populations no policy touches ("All apps"
     vs named-app policies), new admin roles not in the admin policy, guest
     users unhandled.
   - **Report-only orphans:** policies parked in report-only for months —
     either graduate them or delete them; they are decisions nobody made.
4. **Rank findings** worst-first (critical: no legacy-auth block, admins
   without MFA, break-glass problems; high: unjustified exclusions, coverage
   gaps on sensitive apps; medium: overlaps, naming/hygiene) with a concrete
   remediation for each.
5. **Report-only discipline for every change.** Any new or modified policy
   from this review: create in report-only → let it accumulate real sign-in
   data (days, not minutes) → tech reviews the report-only results for
   would-have-blocked legitimate traffic → only then enable, with client
   approval (send_approval) for anything that can block users. What-if tool
   spot checks supplement, never replace, report-only data.
6. **Output.** Post the dated review as a plain-text note (or client-facing
   summary if requested): inventory table, findings ranked with evidence,
   remediation list. Create follow-up tickets for remediations rather than
   burying them in the note, and schedule the next periodic review
   (schedule_ticket).

## Guardrails

- This review changes nothing by itself — it produces findings and remediation
  tickets. Each remediation carries its own approval and report-only cycle.
- Never enable a blocking policy the same day it was written; report-only
  soak is mandatory. When in doubt, leave it in report-only and say so.
- Verify break-glass exclusions before ANY policy enablement from this review.
- Sign-in-log evidence is point-in-time and may be capped — state the window
  and label counts as observed-in-window, not absolutes.
- Do not paste tenant identifiers, full policy JSON exports, or user lists
  into notes destined for PSA sync — summarize, and store the export in the
  client's documentation system (IT Glue/Hudu).
