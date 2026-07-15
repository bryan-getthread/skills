---
name: Liongard Active Directory Read
description: Answer on-prem Active Directory questions from the Liongard AD inspector — privileged group membership, stale accounts, password-policy posture, GPO changes, FSMO roles, and DC health — without a domain login.
category: Liongard Inspectors
tools: [liongard_launchpoint, liongard_metric, liongard_identity, liongard_detection, liongard_timeline, liongard_query, search_tickets, add_ticket_note]
---

# Liongard Active Directory Read

On-prem AD is where MSPs have the least day-to-day visibility and attackers have the most fun. The Liongard Active Directory inspector captures the domain's users, groups, policies, GPOs, and domain-controller layout on every run — so privileged-group audits, stale-account sweeps, and "who changed that GPO" questions get answered from the dataprint instead of an RDP session.

## When to use

- "Who's in Domain Admins at <client>?" / "any new members in privileged groups?"
- "Which AD accounts haven't logged in for 90 days?" / offboarding-verification sweeps
- "What's <client>'s domain password policy?" / accounts with password-never-expires
- "Did a GPO change recently?" / "which DC holds the FSMO roles?" / DC health for a domain incident

## Steps

1. Base pattern first (skills/liongard-inspectors/liongard-access-pattern/SKILL.md): liongard_launchpoint for the client filtered by systemType Active Directory. Verify last run succeeded and date the dataprint. AD inspectors run through an on-prem agent — a failing launchpoint often means the agent host itself is down, which is its own finding.
2. Route the question:
   - Privileged groups → liongard_metric over group membership for Domain Admins, Enterprise Admins, Schema Admins, Administrators, and any client-specific admin groups. Angle shape: `Groups[?Name=='Domain Admins'].Members[]` — verify field paths against the live dataprint. Compare against the last authorized state (previous audit note or docs); every member must be explainable. Unexplained members follow the security/global-admin-audit discipline: cross-check search_tickets for the authorizing change, escalate unclaimed recent additions.
   - Stale accounts → metric reads on enabled users by last-logon timestamp (remember lastLogonTimestamp replicates lazily — treat 90 days as the honest floor, not 14). Cross-check stale hits against offboarding tickets exactly as security/identity-mfa-health-check does; a stale enabled account WITH an offboarding ticket is an incomplete offboarding.
   - Password posture → domain policy fields (min length, complexity, max age, lockout) plus per-account flags: password-never-expires, password-not-required, reversible encryption. Service accounts legitimately carry never-expires — mark "verify purpose," don't auto-flag.
   - GPO / structural changes → liongard_detection and liongard_timeline for GPO modifications, new OUs, trust changes, and DC additions, with timestamps and before/after where captured.
   - Domain plumbing → FSMO role holders, DC list and OS versions, functional levels. A DC on an EOL OS version is a lead for the liongard-windows-workstations EOL sweep and devices-and-infrastructure/warranty-eol-report.
   - Open-ended → liongard_query, then verify with targeted metrics before reporting.
3. Reconcile with cloud identity when the client is hybrid: the same human in liongard_identity may show different MFA/staleness posture on-prem vs. M365 — call out divergence (disabled in AD but active in Entra is a classic offboarding gap).
4. Output: direct answer, evidence rows, dataprint as-of date, replication caveat where last-logon data is involved. Plain-text ticket note on request.

## Guardrails

- Never recommend disabling an account on dataprint staleness alone — corroborate via offboarding ticket, client confirmation, or documented purpose first.
- lastLogonTimestamp is approximate by design; never present it as exact last activity.
- Privileged-group findings lead the output regardless of what was asked.
- Read-only: membership changes, GPO edits, and account disables go to a technician through the client's change process.
- Degradation: no AD inspector → answer from ticket history and documentation only, labeled "not instrumented"; do not guess group membership from memory or stale docs without saying so.
