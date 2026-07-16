---
name: DFS Namespace and Replication
description: Diagnose DFS problems — DFS-N referral failures and users hitting the wrong target, DFS-R replication backlog and conflicts, and staging-quota bottlenecks — from referral order, backlog counts, and DFSR event/health evidence, never by reinitializing replication blindly.
category: Troubleshooting Playbooks
tools: [search_tickets, search_knowledge_base, search_itglue, search_hudu, add_ticket_note, web_search]
---

# DFS Namespace and Replication

Two different systems share the name "DFS" and fail differently. DFS-N (namespaces) is about referrals — which target a user is sent to. DFS-R (replication) is about content getting between targets. This playbook separates them first, then works from referral order or backlog evidence — because a reckless DFS-R reinitialize can lose recent changes on the losing side.

## When to use

- Users open a namespace path and land on the wrong (or a slow/remote) server, or get "no target"
- Files created on one server aren't appearing on another; edits are stale between sites
- DFSR reports a large backlog, is "in error state", or a replicated folder stopped replicating
- Conflicts/losses ("ConflictAndDeleted"), or the staging area / staging quota is a bottleneck

## Steps

1. **Layout and version first.** search_itglue / search_hudu / search_knowledge_base for the DFS design: the namespace(s) and folder targets (which servers back each path), whether targets are referral-ordered by site/cost, the replication groups and their topology (hub-spoke vs full-mesh), the replicated-folder paths, staging-quota sizes, and Windows Server version. Establish whether the complaint is a **referral** problem or a **replication** problem — they're separate. If a Liongard AD/Windows inspector runs, corroborate via liongard_launchpoint / liongard_metric and note dataprint age.
2. **History first.** search_tickets for this client + DFS/file shares: a recent server add/remove/rename, a large data migration (DFSR backlogs balloon after bulk changes), a disk-full or staging event, or an unexpected reboot (a dirty shutdown can force a DFSR recovery). Sudden onset after a change names the cause.
3. **Get the evidence before acting.** DFS-N: `dfsutil` referral output / the management console — which targets exist for the path, their referral order and enabled/online state, and whether targets are actually reachable. DFS-R: the **backlog** count between the specific sending/receiving members (`dfsrdiag Backlog`), the DFSR health report, and the DFSR event log for state (recovery, error, staging-full events). Read the actual backlog/state — not "replication is broken".
4. **Branch:**
   1. **DFS-N referral problems** — users hit the wrong/slow target or none: check target priority/ordering (should honor site cost so users use the local target), whether a target is disabled or its server offline, and client-site awareness (a client in the wrong AD site gets wrong referrals). Fix the referral order/target state; a "no target" often means every target for that folder is offline or the folder target was removed.
   2. **DFS-R backlog** — content is stale because changes are queued: read whether the backlog is *draining* (a big migration replicating out — patience, and possibly a staging-quota bump) vs *stuck* (an error state, a member unreachable, or content-freshness expired). A member down longer than the MaxOfflineTimeInDays becomes stale and needs deliberate recovery — don't just re-enable it. Escalate when: a member has been offline past the content-freshness limit — reconnecting it wrong can resurrect deleted files or lose changes.
   3. **Conflicts / losses** — simultaneous edits on two members create a conflict; DFSR keeps the last-writer and moves the loser to ConflictAndDeleted (recoverable for a time, not forever). This is expected behaviour for multi-master editing, not a bug — the durable answer is often a namespace/locking design change (single writable target, or per-site folders), not "fixing" DFSR. Never treat DFSR as a two-way sync for actively co-edited files.
   4. **Staging quota bottleneck** — replication crawls or errors under heavy change because the staging area is too small (staging must fit the largest files/churn) or the staging disk is full. Read staging events; enlarging the quota is the tuning lever, but confirm disk space and the churn source first.
5. **Verify and note.** Success is a real test: a file created on one member appears on the other within expectations, the backlog is at/near zero (or draining as expected), referrals send a test client to the correct local target. Plain-text note: DFS-N vs DFS-R, the evidence (referral order / backlog / events), branch, action or handoff, verification.

## Guardrails

- No remote execution — dfsutil/dfsrdiag and console steps are guidance for a tech with the right access; use get_ninjaone_device_link to reach a member server when the RMM integration is enabled.
- **Never reinitialize / re-create replication or delete the DFSR database as a first move** — an authoritative/non-authoritative sync resets one side to the other and can lose recent changes on the losing member; understand which member is authoritative and get sign-off first. This is a data-integrity decision.
- Don't reconnect a long-offline member that's past content-freshness without deliberate recovery — it can resurrect deleted files or cause conflicts.
- DFSR is not a real-time two-way sync for co-edited files — if the real problem is concurrent editing, fix the design (single-writer / SharePoint-OneDrive), don't blame replication.
- Do not invent dfsrdiag/dfsutil syntax, event IDs, or offline-limit values — web_search Microsoft's docs and cite (defaults change by version).
- Docs/Liongard coverage varies per tenant — note what you couldn't check and dataprint age. Notes destined for a PSA sync: plain text, no markdown or emojis.
