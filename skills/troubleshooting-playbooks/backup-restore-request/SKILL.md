---
name: Backup Restore Request
description: Intake and work a restore request — deleted files, prior versions, whole mailboxes or servers — pinning down exactly what/when/where, being honest about RPO limits, and verifying the restored data with the requester.
category: Troubleshooting Playbooks
tools: [search_tickets, search_knowledge_base, search_itglue, search_hudu, send_approval, add_ticket_note, web_search]
---

# Backup Restore Request

A restore done from a vague request restores the wrong thing to the wrong place — sometimes over the right thing. This playbook makes the intake precise (what, as-of-when, restored to where), sets honest recovery-point expectations before work starts, and ends with the requester verifying the data, not the tech assuming it.

## When to use

- "<user> deleted a file/folder — can we get it back?"
- "We need <share/mailbox/site> as it was last Tuesday"
- Ransomware/corruption recovery point requests (pair with security response)
- Any request naming a backup product, snapshot, or "previous version"

## Steps

1. **History first.** search_tickets for this data/user — a related deletion or migration ticket pins the loss time better than memory, and a recent identical restore documents the working procedure.
2. **Docs second.** search_itglue / search_hudu / search_knowledge_base for what protects this data: the backup product and scope (is this share/mailbox/VM actually in a job?), schedule/retention, and any restore runbook. Also check the cheap paths first-class: recycle bins, M365 retention/deleted-item recovery, VSS previous versions — many "restores" never need the backup product.
3. **Intake — pin all three axes before touching anything:**
   - **What:** exact path/mailbox/object, and whether it's the item, a folder, or a whole system. Ambiguity here is where wrong restores come from — confirm the precise path with the requester.
   - **When:** the as-of moment — "before it was deleted/changed", with the best-known timestamp. The restore point chosen is the newest point *before* that moment.
   - **Where:** restore to original location (risks overwriting current data — see guardrails) or to a side location for the requester to pick from (the safe default).
4. **RPO honesty — before promising anything.** Read the actual available restore points from the job history, not the schedule: the promise is the *last successful* backup before the loss, and if last night's job failed, the honest answer may be 48+ hours of loss. State plainly: anything created or changed between that restore point and the loss is not recoverable from backup. If the data was never in a job's scope (step 2), say that immediately and check the secondary paths (recycle bin, sync client versions, email copies) rather than raising false hope.
5. **Verify authority.** Restores expose data as-of a past date and can overwrite the present. Confirm the requester is entitled to this data (owner, their manager, or documented authorized contact); anything restoring one user's data at another's request, or company-wide data, goes through send_approval to the authorized client contact. Ransomware-context restores also confirm with the security lead that the environment is clean before restoring into it — restoring into an infected environment loses the restore too.
6. **Execute per the product's procedure.** The tech performs the restore per the vendor's documented procedure (from the client runbook or the vendor's current docs via web_search). Default to side-restore; original-location overwrites require explicit requester acknowledgment of what current data gets replaced. Large restores: give the realistic duration and set the expectation now, not at hour three.
7. **Restore verification — the requester closes it, not the tech.** The requester opens the actual restored data and confirms it is the right version and complete (files open, mailbox items present, application data consistent). "The job said success" is not verification. Only then clean up any side-restore staging.
8. **Note and aftermath.** Plain-text note: what/when/where as intaken, restore point used and why, RPO gap communicated, approvals, verification by whom. If the job history showed failures or the data wasn't protected, open the follow-up ticket to fix coverage — that finding must not die with this ticket.

## Guardrails

- Never restore over the original location without explicit acknowledgment of what gets overwritten — side-restore is the default; when in doubt, side-restore.
- RPO honesty is mandatory and up-front: promise only the last successful restore point, and name the gap. Never imply backup can produce data from between points or from outside job scope.
- Authority check before exposing or restoring data that isn't the requester's own — send_approval for cross-user or bulk restores; deletion-of-backups requests are out of scope entirely and get escalated.
- The requester verifies the restored data before the ticket resolves — job success alone never closes a restore ticket.
- No remote execution — the tech drives the backup product; this playbook supplies sequence, gates, and expectations.
- Coverage gaps discovered en route (failed jobs, unprotected data) are flagged as their own follow-up — silently fixing nothing is the worst outcome.
- Docs tools vary per tenant — note what you could not check.
