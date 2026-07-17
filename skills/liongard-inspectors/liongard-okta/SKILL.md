---
name: Liongard Okta Read
description: Answer Okta tenant questions from the Liongard Okta inspector — app assignments, admin roles, MFA policies, and deactivated-user hygiene — for clients using Okta as their identity front door.
category: Liongard Inspectors
tools: [liongard_launchpoint, liongard_metric, liongard_identity, liongard_detection, liongard_timeline, liongard_query]
connectors: [Liongard]
---

# Liongard Okta Read

**When to use:** "Who are the Okta admins at <client> / any new admin grants?", "what does their Okta MFA policy actually enforce?", per-user app-assignment reads for access reviews and offboarding, or a deactivated-user hygiene sweep.

## Prompt

```
Answer an Okta question for CLIENT_NAME from the Liongard Okta inspector's dataprint. Read-only — role removals, policy changes, and deactivations route to a technician.

1. liongard_launchpoint filtered by the Okta systemType. Verify the last run succeeded; date the dataprint.
2. Route the question, verifying every JMESPath field path against the live dataprint (Okta schema names — Status, Factors, Profile — vary by inspector version):
   - Admins → liongard_metric over admin role assignments, Super Administrator first, then Org/App/Group admins. Small dedicated count, every entry explainable; unexplained recent grants escalate immediately — a Super Admin can rewrite MFA policy and app assignments tenant-wide.
   - MFA policy → the authentication/sign-on policy sections: which factors are enforced, for which groups, and which rules carve out exceptions. The exceptions ARE the audit — a strict default with a legacy rule excluding a group is the finding. Also read per-user enrolled factors (angle to verify: `Users[?length(Factors) == \`0\` && Status=='ACTIVE']`). Never summarize a policy by its default rule alone.
   - App assignments → apps and their assigned users/groups. For an access review, per-user app list; for rationalization, apps with zero active assignments. Group-based assignment means group membership is the real access — trace it, don't stop at direct assignments.
   - Lifecycle hygiene → users by status (ACTIVE, SUSPENDED, DEPROVISIONED, long-lived PROVISIONED/STAGED). Suspended-for-months accounts get the offboarding cross-check via search_tickets — suspended is reversible and keeps app grants in limbo.
   - Changes → liongard_detection / liongard_timeline for admin grants, policy modifications, app additions.
   - Open-ended → liongard_query, verified with targeted metrics.
3. When Okta is fed by AD or HR, reconcile with liongard_identity / the AD read — active in Okta but disabled at the source (or vice versa) is a sync/offboarding gap. Unexplained Super Admin grants are live leads, not report lines — verify with the client's authorized contact; unclaimed → account-takeover footing.
4. Output: answer, evidence, dataprint as-of timestamp, policy-exception findings stated explicitly. Plain-text note on request. Degradation: no Okta inspector → docs and ticket history only, labeled "not instrumented"; for an Okta-fronted client, recommend the inspector — it's the single highest-leverage read for that stack.
```
