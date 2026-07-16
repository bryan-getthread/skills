---
name: Veeam Restore Operations
description: Run a Veeam restore request end-to-end — choosing the right restore type (file-level vs application-item vs full-VM vs Instant Recovery), picking the correct restore point, restoring to a safe location, and verifying the data — distinct from diagnosing why a backup failed (backup-failure-triage / backup-restore-request for intake).
category: Troubleshooting Playbooks
tools: [search_tickets, search_knowledge_base, search_itglue, search_hudu, send_approval, add_ticket_note, web_search]
---

# Veeam Restore Operations

This is the execution playbook for a Veeam restore: once the request is intaken (what/when/where — see backup-restore-request) and the environment is known good, this picks the **right restore type** for the ask, the right restore point, a safe target, and verifies the result with the requester. It is the counterpart to backup-failure-triage, which is about backups that didn't run — here the backups exist and someone needs data back.

## When to use

- A confirmed restore is needed from Veeam and you're choosing/executing the restore method
- "Get back this file/folder" (file-level), "this mailbox item / SharePoint item" (application-item), or "this whole VM"
- A server is down and the business needs it running fast (Instant Recovery candidate)
- After a failed change or ransomware-clean confirmation, restoring a system to a known-good point

## Steps

1. **Confirm intake and environment first.** Ensure the what/when/where and RPO honesty from backup-restore-request are settled, and confirm the environment is safe to restore into (for ransomware context, the security lead confirms it's clean — restoring into an infected environment loses the restore). search_itglue / search_hudu / search_knowledge_base for the Veeam setup: version/edition, the backup jobs and what they protect, repository locations (and whether an immutable/offsite copy exists), and any documented restore runbook. If a Liongard Veeam inspector runs, corroborate job/restore-point state via liongard_launchpoint / liongard_metric and note dataprint age.
2. **History first.** search_tickets for this client + Veeam/restore: a prior identical restore documents the working procedure and duration; a recent job-failure ticket may mean the newest restore point isn't what the requester assumes.
3. **Read the actual restore points before promising.** In Veeam, the real available restore points for the specific object (not the schedule) — the chosen point is the newest good one *before* the loss moment. Confirm the point exists, is not corrupt, and (for application items) that application-aware processing was on for that job so item-level restore is even possible. State the RPO gap plainly if the newest usable point predates the loss.
4. **Choose the right restore type — this is the core decision:**
   1. **File-level recovery** — deleted/changed files or folders: mount the restore point and recover the specific items to a **side location** by default (never overwrite the live path without explicit acknowledgment). Smallest, safest, most common.
   2. **Application-item recovery** (Veeam Explorers) — a mailbox item, a SharePoint/OneDrive item, a SQL/AD/Oracle object: use the matching Explorer, which needs the source job to have been application-aware. Restore the specific item to a safe target; for AD/SQL objects, coordinate with the app owner.
   3. **Full-VM restore** — the whole machine is lost/corrupt: restore the VM, by default to a **new name/location or isolated network** to avoid IP/name collisions and overwriting a machine that might still be needed. Overwriting the original is a deliberate, acknowledged choice.
   4. **Instant Recovery** — the business needs the server *now*: run the VM directly from the backup repository for speed, then migrate it to production storage once verified. It trades performance for uptime and holds the repository busy — plan the follow-through (Storage vMotion / migrate) so it doesn't run indefinitely from backup.
   Pick the smallest type that satisfies the ask — don't full-VM-restore for one file.
5. **Verify authority, then execute per Veeam's procedure.** Cross-user or company-wide restores go through send_approval to the authorized client contact; confirm the requester is entitled to this data. Execute per the vendor's documented procedure (runbook or Veeam's current docs via web_search). Default to side/new-location; original overwrites require explicit acknowledgment of what's replaced. Give realistic duration for large restores up front.
6. **Restore verification — the requester closes it.** The requester opens the actual restored data (files open, mailbox items present, the VM boots and the app works) and confirms it's the right version and complete. "Veeam said success" is not verification. For Instant Recovery, verify *before* and complete the migration to production storage; only then clean up mounts/staging.
7. **Note and aftermath.** Plain-text note: restore type and why, restore point used, target (side vs original), approvals, verification by whom, and (for Instant Recovery) that migration completed. If the restore points revealed a coverage/failure gap, open the follow-up per backup-failure-triage.

## Guardrails

- Never restore over the original location without explicit acknowledgment of what gets overwritten — side/new-location is the default; when in doubt, side-restore.
- Match the restore type to the ask (smallest sufficient) — full-VM or Instant Recovery for a single file is wrong and risky.
- Instant Recovery runs the VM *from the backup repository* — it must be migrated to production storage after verification; never leave it running indefinitely on the repo, and know it loads that repository.
- Ransomware/clean-room context: security confirms the environment is clean before restoring into it, and prefer restoring from an immutable/offsite copy — pair with the security response playbooks.
- Authority check + send_approval for cross-user or bulk restores; the requester verifies before the ticket resolves — job success alone never closes it.
- No remote execution — the tech drives Veeam per its procedure; this playbook supplies the decision tree, gates, and verification. Deletion of backups/restore points is out of scope and escalated.
- Do not invent Veeam menu paths, Explorer capabilities, or version behaviours — web_search Veeam's current docs and cite. Docs/Liongard coverage varies — note what you couldn't check and dataprint age. Notes destined for a PSA sync: plain text, no markdown or emojis.
