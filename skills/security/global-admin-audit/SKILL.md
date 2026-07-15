---
name: Global Admin Audit
description: Audit a client's global/company administrator accounts and recent admin-role changes — flag unexpected admins, missing MFA, and grants with no authorizing ticket.
category: Security
tools: [liongard_identity, liongard_detection, liongard_timeline, search_tickets, add_ticket_note]
---

# Global Admin Audit

Count the keys to the kingdom and account for every one of them: who holds top-level admin, who granted admin recently, and which of those facts nobody can explain.

## When to use

- "Who has global admin at <client>?" / periodic admin review
- After an incident, to verify no privilege escalation persisted
- Feeding a posture review, insurance form, or compliance evidence request

## Steps

1. Enumerate current holders of global/company administrator and equivalent top-tier roles via liongard_identity. Compare the count to the working benchmark: a small number of dedicated admin accounts (typically 2–4) plus a documented break-glass account — not day-to-day user accounts wearing admin.
2. Pull recent admin-role changes: grants and removals from liongard_detection and liongard_timeline where available, with actor, target, and timestamp.
3. Authorized-change cross-check for every current admin and every recent grant: search_tickets for the change, onboarding, or project ticket that explains it. This is the same discipline as the new-user-created-alert skill — the boring explanation gets checked before the alarming one.
4. Flag, ranked:
   - Admins or grants with NO authorizing ticket and no client confirmation — unexplained privilege.
   - Admin accounts without MFA — critical, regardless of explanation.
   - Shared or generic admin accounts (no individual accountability).
   - Day-to-day user accounts holding standing admin (candidates for separate admin accounts or just-in-time elevation).
   - Break-glass account missing, undocumented, or used for routine work.
5. An unexplained RECENT grant is not a report line — it is a live lead: verify with the client's authorized contact immediately, and if unclaimed, treat the granting account as suspected compromised and escalate to the account-takeover-runbook rather than finishing the report.
6. Output per-client findings, flagged items first, each with evidence and a recommendation. Document what was checked and the decision reasoning in the note.

## Guardrails

- Cross-check before alarm — most unexpected admins are undocumented-but-legitimate; say "unexplained," not "unauthorized," until the client confirms.
- Never close the loop on assumption: an admin nobody claims stays flagged until someone with authority claims it.
- An unclaimed recent grant escalates immediately — when in doubt, escalate; a false escalation is cheap, a missed compromise isn't.
- Report and recommend only; role removals go through the client's change process.
- Degradation: Liongard absent → the audit can only cover what tickets and the client attest; label the output "attestation-based, not instrumented."
