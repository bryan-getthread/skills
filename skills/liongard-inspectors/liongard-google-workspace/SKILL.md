---
name: Liongard Google Workspace Read
description: Answer Google Workspace tenant questions from the Liongard inspector — super admin roles, 2SV coverage, license usage, and Drive-sharing posture — for clients living in Google instead of Microsoft.
category: Liongard Inspectors
tools: [liongard_launchpoint, liongard_metric, liongard_identity, liongard_detection, liongard_timeline, liongard_query]
connectors: [Liongard]
---

# Liongard Google Workspace Read

**When to use:** "Who are the super admins in <client>'s Google Workspace?", 2SV coverage / MFA posture for a Google-shop client, license usage vs. paid, or external Drive-sharing posture before a security review.

## Prompt

```
Answer a Google Workspace question for CLIENT_NAME from the Liongard Google Workspace inspector's dataprint. Read-only — role, 2SV, and license changes route to a technician.

1. liongard_launchpoint filtered by the Google Workspace systemType. Verify the last run succeeded; date the dataprint.
2. Route the question, verifying every JMESPath field path against the live dataprint (Google schemas differ from Microsoft — do NOT transplant M365 paths):
   - Admin roles → liongard_metric over user role assignments, super admins first (angle to verify: `Users[?IsAdmin==\`true\`].PrimaryEmail`). Every super admin explainable; unexplained recent grants escalate. liongard_identity gives the reconciled cross-system view when the client also runs AD or M365.
   - 2SV coverage → enrolled vs. not per user, AND whether 2SV is enforced or merely allowed at domain/OU level — that distinction IS the finding ("allowed but 62% enrolled" is a policy gap). Privileged accounts without 2SV are critical and lead the output. Never blur "2SV enforced" and "2SV enrolled" — say which one the data supports.
   - Licenses → SKU assignments vs. total, suspended-but-licensed accounts (money leaking), archived-user licensing where captured.
   - Drive sharing → domain sharing settings (external sharing on/off, link-sharing defaults, whitelisted domains). Setting-level only — per-file sharing audits are out of the dataprint's reach; say so rather than implying coverage.
   - Stale accounts → active users by last-login where captured; cross-check search_tickets for offboarding before recommending suspension.
   - Changes → liongard_detection / liongard_timeline for admin grants, setting flips, user lifecycle events.
   - Open-ended → liongard_query, verified before reporting.
3. Super-admin findings lead regardless of the question. Output: answer, evidence, dataprint as-of timestamp, enforcement-vs-enrollment distinction stated wherever 2SV appears. Plain-text note on request. Degradation: no Google Workspace inspector → docs and ticket history only, labeled "not instrumented"; for a Google-primary client, flag the gap to account management.
```
