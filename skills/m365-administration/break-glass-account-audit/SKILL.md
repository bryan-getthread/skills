---
name: Break-Glass Account Audit
description: Verify a tenant's emergency-access accounts actually work when everything else is broken — CA exclusions on every policy, sealed credentials, sign-in alerting, and a quarterly test sign-in. Use for periodic break-glass audits, before any CA change, or when nobody can say where the emergency credentials are.
category: M365 Administration
tools: [search_tickets, search_knowledge_base, search_itglue, search_hudu, add_ticket_note, update_ticket, create_ticket, schedule_ticket, send_approval, web_search]
---

# Break-Glass Account Audit

A break-glass account is insurance that only pays out if it was maintained:
excluded from every policy, credentials actually retrievable, alerts firing
on use, and tested recently enough to trust. This audit verifies all four —
because the discovery moment for a broken break-glass account is a tenant
lockout.

## When to use

- Quarterly emergency-access audit for a managed tenant.
- Before enabling or changing any Conditional Access policy (paired with
  conditional-access-review and security-defaults-vs-ca migrations).
- "Where are <client>'s emergency admin credentials?" and nobody knows.
- After an incident or lockout where break-glass was (or should have been)
  used.

## Steps

1. **Existence and construction.** Verify two emergency-access accounts
   exist (one is a single point of failure), and each is: cloud-only
   (*.onmicrosoft.com — not federated, not synced from on-prem, so an AD or
   federation outage can't kill them), holding Global Administrator, not
   assigned to any individual's daily identity, and with licensing/naming
   per the MSP standard (search_knowledge_base / search_itglue for the
   documented standard). Missing or mis-built accounts → remediation tickets
   (create_ticket), not silent fixes.
2. **Exclusion sweep — every policy, every time.** Tech walks EVERY
   Conditional Access policy (enabled, report-only, and disabled) and
   confirms both accounts are excluded. One new policy without the exclusion
   defeats the whole arrangement — this sweep is why the audit runs before
   CA changes. Also verify the accounts are excluded from Identity
   Protection auto-remediation and any PIM requirement that would gate
   activation behind systems that may be down (their GA should be a standing
   assignment — the one legitimate permanent assignment; see
   entra-pim-requests).
3. **Credential custody.** Verify the credentials are stored per the
   client's documented procedure — sealed (safe, sealed envelope, or vault
   with dual control), split from each other, and NOT dependent on one
   person's phone or an MFA method that dies with a single device. FIDO2
   keys stored in separate secure locations are the strong pattern; a
   long sealed password is the acceptable floor. The audit confirms the
   storage reference and who can access it — never the credentials
   themselves, which appear in no ticket, note, or doc.
4. **Usage alerting.** Verify an alert fires on ANY sign-in by these
   accounts (log-based alert to a monitored destination). A break-glass
   sign-in is either a drill or an emergency — silence is unacceptable.
   Test the alert as part of step 5. Review sign-in history since the last
   audit: any use outside a documented drill/emergency is a security
   escalation, immediately.
5. **The quarterly test — actually sign in.** A controlled test: retrieve
   credentials per procedure, sign in, confirm GA access works, confirm the
   alert fired, sign out, and re-seal/rotate per procedure. Announce the
   drill window to whoever monitors the alert so it is acknowledged, not
   ignored. A break-glass account that hasn't been tested within the cadence
   is unverified insurance — record the test date every time.
6. **Document and schedule.** Plain-text note: both accounts verified
   (construction, exclusions with policy count checked, custody reference,
   alerting test result, drill result and date), findings with remediation
   tickets, and the next audit scheduled (schedule_ticket, quarterly).
   Update the client's break-glass procedure doc in IT Glue/Hudu if
   anything changed.

## Guardrails

- Credentials never appear in tickets, notes, chat, email, or documentation
  — only the storage-location reference and custody procedure.
- Any unexplained break-glass sign-in is treated as a compromise until
  proven a drill — escalate per the client's security process before
  continuing the audit.
- Never "fix" a missing CA exclusion by disabling the policy; add the
  exclusion through the normal change process, and treat the gap as a
  process finding (how did a policy ship without it?).
- The test sign-in is a controlled, announced drill with re-seal/rotation
  afterward — an untracked casual login by whoever holds the envelope is
  itself a finding.
- These accounts are exempt from stale-account cleanup and MFA-enforcement
  sweeps by design — ensure hygiene automations (see identity-mfa-health-check,
  stale-device-cleanup patterns) know to exclude them, and document why.
