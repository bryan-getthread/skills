---
name: Liongard Okta Read
description: Answer Okta tenant questions from the Liongard Okta inspector — app assignments, admin roles, MFA policies, and deactivated-user hygiene — for clients using Okta as their identity front door.
category: Liongard Inspectors
tools: [liongard_launchpoint, liongard_metric, liongard_identity, liongard_detection, liongard_timeline, liongard_query, search_tickets, add_ticket_note]
---

# Liongard Okta Read

When a client fronts everything with Okta, the Okta tenant is the identity blast radius: whoever controls it controls every downstream app. The Liongard Okta inspector's dataprint answers who administers it, what MFA policy actually applies, who is assigned to which app, and whether offboarded users are truly gone.

## When to use

- "Who are the Okta admins at <client>?" / "any new admin grants?"
- "What does their Okta MFA policy actually enforce?" / factor enrollment coverage
- "Which apps does <user> have through Okta?" / app-assignment reads for access reviews and offboarding checks
- "Are their deactivated users cleaned up?" — suspended-vs-deactivated hygiene sweep

## Steps

1. Base pattern (skills/liongard-inspectors/liongard-access-pattern/SKILL.md): liongard_launchpoint filtered by the Okta systemType. Verify last run succeeded; date the dataprint.
2. Route the question:
   - Admins → liongard_metric over admin role assignments: Super Administrator first, then Org/App/Group admins. Apply the security/global-admin-audit discipline — small dedicated count, every entry explainable, unexplained recent grants escalate immediately. In Okta the stakes compound: a Super Admin can modify MFA policy and app assignments tenant-wide.
   - MFA policy → the authentication/sign-on policy sections: which factors are enforced, for which groups, and which policy rules carve out exceptions. The exceptions ARE the audit: a strict default policy with a legacy rule excluding a group is the finding. Also read per-user enrolled factors; angle shape: `Users[?length(Factors) == \`0\` && Status=='ACTIVE']` — verify field paths against the live dataprint. Deep identity work hands off to security/identity-mfa-health-check.
   - App assignments → apps and their assigned users/groups. For an access review: per-user app list. For app rationalization: apps with zero active assignments (license/attack-surface cleanup candidates). Group-based assignment means group membership is the real access — trace it, don't stop at direct assignments.
   - Lifecycle hygiene → users by status: ACTIVE, SUSPENDED, DEPROVISIONED, and long-lived PROVISIONED/STAGED accounts that never activated. Suspended-for-months accounts get the offboarding cross-check (search_tickets) — suspended is reversible and keeps app grants in limbo; the finding is "suspended since <date>, offboarding ticket says departed."
   - Changes → liongard_detection / liongard_timeline for admin grants, policy modifications, and app additions in the window of interest.
   - Open-ended → liongard_query, verified with targeted metrics.
3. Reconcile with the upstream directory when Okta is fed by AD or HR: a user active in Okta but disabled at the source (or vice versa) is a sync/offboarding gap — pair with the liongard-active-directory read for the comparison.
4. Output: answer, evidence, dataprint as-of timestamp, policy-exception findings stated explicitly. Plain-text note on request.

## Guardrails

- Policy reads report what the policy enforces, exceptions included — never summarize a policy by its default rule alone.
- Unexplained Super Admin grants are live leads, not report lines: verify with the client's authorized contact; unclaimed → security/account-takeover-runbook footing.
- Read-only: role removals, policy changes, and deactivations route to a technician through the client's change process.
- Verify field paths against the live dataprint; Okta schema names (Status, Factors, Profile) vary by inspector version.
- Degradation: no Okta inspector → docs and ticket history only, labeled "not instrumented"; for an Okta-fronted client, recommend the inspector — it is the single highest-leverage read for that stack.
