---
name: Liongard Google Workspace Read
description: Answer Google Workspace tenant questions from the Liongard inspector — super admin roles, 2SV coverage, license usage, and Drive-sharing posture — for clients living in Google instead of Microsoft.
category: Liongard Inspectors
tools: [liongard_launchpoint, liongard_metric, liongard_identity, liongard_detection, liongard_timeline, liongard_query, search_tickets, add_ticket_note]
---

# Liongard Google Workspace Read

The Google-shop equivalent of the M365 tenant read: interrogate a client's Google Workspace through the Liongard inspector's dataprint for admin roles, 2-Step Verification coverage, license consumption, and sharing posture. Most MSP tooling is Microsoft-first, so for Google clients this inspector is often the only instrumented view the desk has.

## When to use

- "Who are the super admins in <client>'s Google Workspace?"
- "What's their 2SV coverage?" / MFA posture for a Google-shop client
- "How many Workspace licenses are they paying for vs. using?"
- "Can their users share Drive files externally?" / sharing posture before a security review

## Steps

1. Base pattern (skills/liongard-inspectors/liongard-access-pattern/SKILL.md): liongard_launchpoint filtered by the Google Workspace systemType for the client. Verify last run succeeded; date the dataprint.
2. Route the question:
   - Admin roles → liongard_metric over user role assignments: super admins first, then delegated admin roles. Angle shape: `Users[?IsAdmin==\`true\`].PrimaryEmail` — verify field paths against the live dataprint. Apply the security/global-admin-audit discipline: every super admin explainable, small dedicated-count benchmark, unexplained recent grants escalate. liongard_identity gives the reconciled cross-system view when the client also runs AD or M365.
   - 2SV coverage → enrolled vs. not per user, and whether 2SV is enforced or merely allowed at the domain/OU level — the distinction is the finding: "allowed but 62% enrolled" is a policy gap, not a user problem. Privileged accounts without 2SV are critical and lead the output. Deep identity work hands off to security/identity-mfa-health-check.
   - Licenses → SKU assignments vs. total, suspended-but-licensed accounts (money leaking), archived-user licensing where captured.
   - Drive sharing → domain sharing settings (external sharing on/off, link-sharing defaults, whitelisted domains) as captured. Setting-level reads only — per-file sharing audits are out of the dataprint's reach; say so rather than implying coverage.
   - Stale accounts → active users by last-login where captured; cross-check against offboarding tickets (search_tickets) before recommending suspension, same as the AD and M365 reads.
   - Changes → liongard_detection / liongard_timeline for admin grants, setting flips, and user lifecycle events.
   - Open-ended → liongard_query, verified before reporting.
3. Output: answer, evidence, dataprint as-of timestamp, enforcement-vs-enrollment distinction stated wherever 2SV is discussed. Plain-text note on request.

## Guardrails

- Enforcement vs. enrollment is never blurred — "2SV enforced" and "2SV enrolled" are different claims and the output says which one the data supports.
- Verify JMESPath field paths against the live dataprint; Google inspector schemas differ from the Microsoft ones — do not transplant M365 paths.
- Super-admin findings lead regardless of the question asked.
- Read-only: role changes, 2SV enforcement, and license changes route to a technician via the client's change process.
- Degradation: no Google Workspace inspector → docs and ticket history only, labeled "not instrumented"; for a Google-primary client this gap is worth flagging to account management.
