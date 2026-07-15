---
name: Offboarding Completeness Audit
description: Post-offboarding sweep for anything missed — licenses still assigned, live delegations, unreturned devices, lingering MFA methods and sessions, external shares. Use after an offboarding closes, or on a periodic sweep of recent departures.
category: Onboarding & Access
tools: [search_tickets, search_contacts, search_clients, search_itglue, search_knowledge_base, add_ticket_note, create_ticket, update_ticket, log_time_entry]
---

# Offboarding Completeness Audit

Catches what the offboarding missed while it is still cheap to fix: a
checklist-driven sweep of a departed user's residual footprint, with findings
raised as actionable follow-ups instead of a someday list.

## When to use

- "Double-check <user>'s offboarding — did we get everything?"
- A weekly/monthly sweep of departures in the last N days.
- Before a client's security review or license true-up.
- An offboarding closed quickly under pressure ("terminated, cut access now")
  and deserves a calm second pass.

## Steps

1. Identify the departed user(s) and pull the offboarding ticket(s)
   (search_tickets, search_contacts) plus the client's offboarding SOP
   (search_knowledge_base / search_itglue) as the baseline of what should have
   happened.
2. Sweep the residual footprint against the checklist, verifying each from
   evidence, not from the offboarding ticket's claims:
   - Account: sign-in actually disabled; sessions revoked; password reset.
   - MFA: no registered methods remain; no app passwords or trusted devices.
   - Licenses: none still assigned to the user (billing leak — cross-check
     License Lifecycle notes and the billing view).
   - Mailbox: converted/forwarded per policy BEFORE license removal; no
     forwarding rules pointing to external personal addresses.
   - Delegations: no surviving Full Access / Send As grants held BY the user
     on other mailboxes, and delegations TO the departed mailbox reviewed.
   - Groups and DLs: removed from security groups and distribution lists.
   - Devices: hardware returned or tracked; managed-device enrollment
     disabled/retired; no active remote-access tooling.
   - External shares: file/folder sharing links owned by the user, and any
     third-party app grants (OAuth consents) still alive.
   - AD hygiene: object disabled, ticket number stamped in the description,
     moved to the disabled OU per policy, AD Connect sync reflected.
3. Classify each finding: SECURITY (live access path — sessions, MFA method,
   delegation, external forward) vs COST (license still burning) vs HYGIENE
   (naming, OU, documentation).
4. For SECURITY findings, flag immediately on the offboarding ticket and to
   the client's contact per policy; for the rest, create or update follow-up
   tickets (create_ticket) with owners rather than a passive list.
5. Post a plain-text audit note: user, offboarding ticket reference, each
   checklist item PASS / FAIL / UNVERIFIABLE (with why), findings by class,
   and the follow-up ticket references. State clearly which items could not be
   verified with available access — an unverifiable item is not a pass. Log
   time (log_time_entry).

## Guardrails

- Verify from evidence; never mark an item PASS because the offboarding note
  said it was done.
- This audit is read-and-report: it opens follow-ups but does not itself
  revoke, delete, or convert anything — remediation runs through the owning
  skill with its own confirmations.
- Never write "breach" or "compromised" for a residual access path; report it
  as an open item requiring closure (defensive writing).
- UNVERIFIABLE is an honest verdict — say what access would be needed to check.
- Do not include credentials or personal data beyond what the finding requires.

## Unattended (Flows) variant

- Your entire reply is posted verbatim as the audit note — plain text, no
  narration, no questions.
- Report findings only; create no tickets and change nothing. If the departed
  user or offboarding ticket cannot be identified with confidence, output
  nothing and stop. When in doubt, do nothing.
