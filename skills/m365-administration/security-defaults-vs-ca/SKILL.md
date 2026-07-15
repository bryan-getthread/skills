---
name: Security Defaults vs Conditional Access
description: Decide whether a tenant should stay on security defaults or move to Conditional Access — and when migrating, sequence it so there is never an unprotected gap. Use for "should <client> turn off security defaults", small-tenant posture questions, or any request that requires a CA exception on a defaults tenant.
category: M365 Administration
tools: [search_tickets, search_knowledge_base, search_itglue, search_hudu, add_ticket_note, update_ticket, send_approval, schedule_ticket, web_search]
---

# Security Defaults vs Conditional Access

Security defaults are free, all-or-nothing, and better than a badly built CA
set. This skill makes the stay-or-migrate call on licensing and actual need —
and when migration is right, sequences it so protection never has a gap
between "defaults off" and "CA on."

## When to use

- "Should we move <client> off security defaults?"
- A request needs an exception or condition security defaults can't express
  (trusted locations, per-app policy, excluding a service account).
- Small-tenant posture review: "is security defaults enough for them?"
- Someone turned defaults off "temporarily" and nothing replaced them.

## Steps

1. **Establish current state.** Tech confirms whether security defaults are
   on and what CA policies (if any) exist. A tenant with defaults OFF and no
   enforced CA policies is an active incident-grade finding — flag it
   immediately and treat restoring protection as the priority, whatever the
   original ticket asked.
2. **Know what defaults actually give** (verify current behavior via
   web_search — the feature has evolved): MFA registration required for all
   users, MFA enforced for admins and challenged for users, legacy
   authentication blocked, and protection of privileged actions. No
   exclusions, no conditions, no per-app logic — that rigidity is a feature
   for tenants with nobody to maintain policy.
3. **The stay case — recommend defaults when:** the tenant lacks Entra ID P1
   licensing (CA requires it — check actual licenses, not assumptions); no
   real exception needs exist; and there is no ongoing capacity to review CA
   policies (an unmaintained CA set rots — see conditional-access-review;
   defaults never rot). "We might want exceptions someday" is a stay, not a
   migrate.
4. **The migrate case — recommend CA when there is a concrete, current need
   defaults cannot express:** documented exceptions (service accounts,
   conference rooms), named/trusted-location conditions, device-compliance
   or app-protection grants (see intune-compliance-policies,
   app-protection-policies), per-app policies, or phishing-resistant
   requirements for admins. The trigger is a real requirement in hand, plus
   P1 licensing, plus a maintenance commitment (a scheduled periodic
   CA review).
5. **Migration sequence — no-gap rule:**
   1. Break-glass accounts verified first (break-glass-account-audit).
   2. Build CA policies replicating at minimum everything defaults provided:
      MFA for all users, MFA for admins, block legacy auth — plus the new
      requirements that justified migrating.
   3. Report-only soak with real sign-in data reviewed (days, not minutes).
   4. Enable the CA policies.
   5. Only then turn security defaults off — defaults and CA cannot coexist,
      so this is the last step, never the first. The wrong order (defaults
      off, then start building) is the classic gap that gets tenants
      compromised mid-migration.
   6. Post-migration verification: spot-check sign-in logs that policies
      apply; confirm legacy auth is still blocked.
6. **Approval and documentation.** send_approval to the client authority for
   the migration plan (or the stay recommendation, if the ticket asked to
   migrate and the answer is no — pushback is a deliverable). Plain-text
   note: decision, rationale, licensing facts, sequence executed with dates,
   approver, and rollback (re-enable security defaults if the CA set must be
   torn down — crude but instant protection). Schedule the first periodic
   CA review (schedule_ticket) as part of migration completion.

## Guardrails

- Never turn security defaults off before replacement CA policies are built,
  soaked in report-only, and enabled — the no-gap rule is the whole skill.
- Do not migrate a tenant to CA to solve one exception if the client has no
  capacity to maintain policy — a rotting CA set is worse than rigid
  defaults; say so in the recommendation.
- Verify P1 licensing before proposing CA; a migration plan the licenses
  can't support is fiction.
- A defaults-off/no-CA tenant found in the wild is escalated as a security
  finding, not quietly fixed within an unrelated ticket.
- The stay/migrate decision is recorded with rationale and approver — this
  is the tenant's foundational access-control decision and it must be
  auditable (see tenant-onboarding-checklist).
