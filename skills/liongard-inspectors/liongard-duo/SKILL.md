---
name: Liongard Duo Read
description: Answer Duo MFA posture questions from the Liongard Duo inspector — enrollment coverage, bypass users, admin list, and protected-integration inventory — for coverage audits and QBR evidence.
category: Liongard Inspectors
tools: [liongard_launchpoint, liongard_metric, liongard_detection, liongard_timeline, liongard_query, search_tickets, add_ticket_note]
---

# Liongard Duo Read

Posture reads on a client's Duo deployment from the Liongard Duo inspector: who is enrolled, who is in bypass, who administers it, and which applications Duo actually protects. This is the coverage-and-configuration side; live Duo events (fraud pushes, push fatigue, bypass-code requests) are handled by vendor-runbooks/duo-mfa-anomalies — this skill tells you how big the blast radius is, that one handles the explosion.

## When to use

- "What's Duo enrollment coverage at <client>?" / "who still isn't enrolled?"
- "Any users in bypass status?" — the standing-hole audit
- "Who are the Duo admins?" / "which apps are behind Duo?"
- Feeding MFA-coverage numbers to a posture review, QBR, or insurance questionnaire

## Steps

1. Base pattern (skills/liongard-inspectors/liongard-access-pattern/SKILL.md): liongard_launchpoint filtered by the Duo systemType for the client. Verify last run succeeded; date the dataprint.
2. Route the question:
   - Enrollment coverage → liongard_metric over users by status: active-enrolled, pending/not-enrolled, disabled. Angle shape: `Users[?Status=='bypass']` — verify field paths against the live dataprint. Coverage percent = enrolled ÷ active users, and the denominator matters: reconcile the Duo user count against the directory source (liongard-active-directory or liongard-m365-tenant reads) — a 100% Duo enrollment over half the workforce is a 50% finding, not a 100% one.
   - Bypass users → every account in bypass status is a standing MFA hole: for each, cross-check search_tickets for the authorizing request and its intended expiry. Bypass with no ticket, or bypass past its stated window, is a flagged finding. This pairs with vendor-runbooks/duo-mfa-anomalies, which mandates time-boxed, logged bypass codes — this read is the audit that catches the ones that never got cleaned up.
   - Admins → the Duo administrator list, each explainable under the security/global-admin-audit discipline; Duo admins can weaken MFA for everyone, so unexplained entries escalate.
   - Integrations → the protected-application inventory: what is behind Duo (VPN, RDP gateways, M365, LDAP) and — read against the client's documented stack — what is not. The gap list is usually the QBR's most actionable slide.
   - Changes → liongard_detection / liongard_timeline for new bypass grants, admin additions, and integration changes.
   - Open-ended → liongard_query, verified before reporting.
3. Output: coverage numbers with both numerator and denominator stated, bypass and admin findings flagged first, integration gap list, dataprint as-of timestamp. Plain-text note on request.

## Guardrails

- Never report coverage percent without naming the denominator and its source — Duo-only percentages flatter incomplete rollouts.
- Bypass findings are never softened: each is a standing bypass of MFA and reads as such until ticketed and time-boxed.
- Read-only: enrollment pushes, bypass revocation, and admin changes are technician actions in the Duo Admin Panel per vendor-runbooks/duo-mfa-anomalies.
- Verify field paths against the live dataprint before trusting zeros.
- Degradation: no Duo inspector → answer from directory-side MFA data (security/identity-mfa-health-check) and tickets, labeled "Duo not instrumented — coverage from directory view only."
