---
name: Security Lead Monthly Ritual
description: A security lead's month-end runbook — monthly security report cycle, identity/MFA health sweep, global-admin audit, and awareness-campaign status — run when a security lead says "monthly security pass", "run the security month-end", or "security report time".
category: Role Rituals
tools: [search_tickets, search_clients, liongard_identity, liongard_metric]
---

# Security Lead Monthly Ritual

The security lead's month-end: produce the client-facing security reports, sweep identity hygiene across the base, audit who holds the keys, and confirm the human layer (awareness) is actually running.

## When to use

- "Run the security month-end" / "monthly security reports" / "identity hygiene sweep."
- First week of the month, covering the month closed.
- Before security-inclusive QBRs, to have fresh evidence ready.

## Steps

1. **Report cycle (60+ min)** — run `skills/security/monthly-security-report` per client due one this month: incidents, alerts handled, posture movement, recommendations. Batch by tier; managed-security clients first.
2. **Identity/MFA sweep (30 min)** — run `skills/security/identity-mfa-health-check` across the client base: MFA coverage gaps, dormant privileged accounts, risky sign-in patterns. Every gap becomes a ticket or a named exception — a known gap without a ticket is an accepted risk nobody accepted.
3. **Global-admin audit (20 min)** — run `skills/security/global-admin-audit`: who holds global/domain admin per tenant, what changed since last month, and whether each holder still needs it. New admins that no change record explains get investigated, not normalized.
4. **Awareness status (15 min)** — check the awareness program via `skills/security/security-awareness-coordination` and `skills/security/phishing-simulation-program`: campaigns run this month, completion rates, clients overdue for a campaign. Overdue clients get scheduled, not just noted.
5. Output the monthly security card: reports produced (client, one-line verdict), identity gaps → tickets opened, admin-audit deltas and dispositions, and awareness status with next month's schedule.

## Time-short version

Steps 2 and 3 — identity gaps and unexplained admins are the findings that become breaches. Reports and awareness can trail by a week with a note.

## Guardrails

- Findings become tickets, not lists: the ritual is not complete while a critical gap exists only in the output card.
- Admin-audit conclusions require verified data (inspector freshness / tenant read succeeded) — "no data" is reported as no data, never as "no change".
- Client reports contain that client's data only; the cross-client sweep results never leak into an individual client's report.
- Report sends to clients require human review — the cycle drafts, a person approves.
- Skip-with-note beats fake completion; per-client skips are listed with reasons.
