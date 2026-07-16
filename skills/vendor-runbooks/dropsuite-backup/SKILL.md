---
name: Dropsuite Backup
description: A Dropsuite SaaS-backup ticket arrived (M365/Google Workspace mailbox, drive, SharePoint/Teams) — a restore request, a license/seat reconciliation, or a job failure. Verify authorization before restores and reconcile protected seats against real users. Verify against Dropsuite's current documentation.
category: Vendor Runbooks
tools: [search_tickets, search_clients, search_contacts, search_itglue, add_ticket_note, update_ticket]
---

# Dropsuite Backup

Vendor specialization of m365-backup-saas for Dropsuite. m365-backup-saas owns the SaaS-backup canon — authorization-before-restore, license-count reconciliation, and job-failure logic; this skill adds Dropsuite's specifics (per-mailbox/per-user protection model, its M365 and Google Workspace coverage, and its incremental-backup cadence). Verify feature names and console behavior against Dropsuite's current documentation.

## When to use

- Someone asks to restore a Dropsuite-protected mailbox, drive files, SharePoint site, or Teams data "as of <date>"
- Protected-seat counts need reconciling against the tenant's actual users (billing or coverage audit)
- A Dropsuite backup-job failure or "user not protected" condition surfaces

## Steps

1. Restore requests — authorization before mechanics, per m365-backup-saas:
   - Verify the requester per the identity ladder. Restoring ANOTHER person's mailbox/files (including a departed employee's) needs the client's authorized approver on file (search_itglue / search_contacts) — a manager wanting a subordinate's data is a client data-access decision, not a desk favor. Record who authorized it.
   - Scope precisely: which objects, point-in-time target, and destination — Dropsuite supports in-place restore and export/download; in-place can merge/overwrite current data, so state that behavior and prefer non-destructive targets unless in-place is explicitly required.
   - Confirm a restore point exists at the requested date for that object BEFORE promising anything — Dropsuite protection begins when the user was added, and coverage is bounded by when incrementals started for that seat. If the need traces to deletion/ransomware/compromise, branch to the security side first (compromised-account-containment / phishing-triage) so the restore doesn't re-import or mask evidence.
2. License / seat reconciliation — the coverage audit, per m365-backup-saas:
   - Pull three numbers: seats licensed in Dropsuite, users actually configured for protection, and current active users in the tenant (the technician exports the latter from the admin center; the agent reconciles).
   - Classify the gaps — active users NOT protected (the dangerous, daily-accruing exposure, typically new hires when auto-add isn't enabled), protected accounts that no longer exist/are departed (paying for ghosts — but confirm the client's departed-data retention intent before removing, since deleting protection can delete the backups), and licensed-but-unassigned seats.
   - Recommend enabling/verifying Dropsuite's auto-add for new users where supported, aligning seat count at the next billing cycle, and documenting departed-user retention with the client's approver. Route commercial changes to account management.
3. Job failures / unprotected-object conditions → backup-failure-triage logic: the SaaS-specific classic is tenant auth/token expiry (reconsent / service-account fix); also API throttling and object-type limits. Recurrence via search_tickets; end with the exposure statement — last successful backup per affected object.
4. Document: for restores — requester, authorization, scope, point-in-time, destination, result; for reconciliation — the three numbers, gaps by count (not names in client-facing summaries), and recommendations with dates.

## Guardrails

- No restore of another user's data without the client's documented approver — urgency does not waive authorization, and the request channel is not proof of identity.
- Never run/recommend an in-place restore without stating the overwrite/merge consequence; default to export/alternate-location when in doubt.
- Never promise recoverability before confirming a restore point exists for that object at that date — protection-start and retention bound the truth.
- Unprotected active users are an exposure finding with a daily-growing cost — flag loudly, never bury in a billing note.
- Removing protection from departed users may destroy their backups — retention decision first, seat cleanup second.
- "Microsoft/Google retains our data" is not a backup — a client declining SaaS backup is a documented account-management decision, not a triage argument.
- Console actions are technician steps the agent directs and records; results stated per-object with result-cap honesty on large reconciliations.
