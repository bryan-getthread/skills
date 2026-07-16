---
name: SQL Server Performance
description: Diagnose Microsoft SQL Server slowness — blocking and deadlocks, missing or bloated indexes, stale statistics, tempdb contention, plan-cache/parameter-sniffing problems — from live wait stats and the actual query, never from "the database is slow" alone.
category: Troubleshooting Playbooks
tools: [search_tickets, search_knowledge_base, search_itglue, search_hudu, add_ticket_note, web_search]
---

# SQL Server Performance

"SQL is slow" is a symptom, not a diagnosis. This playbook finds the actual bottleneck from wait statistics and the specific slow query before anyone adds an index or restarts a service — and it stops cold at the line where the database backs a vendor's application.

## When to use

- "The database / <LOB app> got slow this morning" or intermittently hangs
- Reports, saved queries, or a specific screen time out; deadlock victim errors (error 1205)
- "Blocking" complaints, one user's action freezing others
- tempdb full, log file growth, or CPU pegged on the SQL host

## Steps

1. **Version and edition first.** search_itglue / search_hudu / search_knowledge_base for the instance: SQL Server version and edition (Express has a hard 10 GB / 1 GB RAM / 1-socket ceiling that alone explains many "slow" cases), the host spec, whether it is shared or dedicated, and whether this database is owned by a vendor's application (see the vendor-caution guardrail). If Liongard's SQL inspector runs for the tenant, read instance config/state via liongard_launchpoint / liongard_metric and note the dataprint age.
2. **History first.** search_tickets for this client + SQL / the app: a recurring end-of-month slowdown, a prior index or maintenance-plan ticket, or a recent app upgrade reframes the problem. Sudden onset on a date points at a change (patch, data growth, job failure), not tuning.
3. **Scope: everything, or one thing.** Whole-instance slow (CPU/memory/IO pressure, tempdb) vs one query/report slow (plan, indexing, parameter sniffing) vs one user blocking others (locking). This fork decides the whole investigation — establish it before theorizing.
4. **Measure, don't guess.** Read live signals, not vibes: current sessions and blocking chains (`sys.dm_exec_requests` / `sys.dm_os_waiting_tasks` — find the head blocker), aggregate wait stats (`sys.dm_os_wait_stats`), the expensive statements (`sys.dm_exec_query_stats` + plan), and for a specific slow query the **actual** execution plan, not the estimated one. Do not recite DMV syntax from memory for the instance's version — web_search and cite.
5. **Branch:**
   1. **Blocking / deadlocks** — find the head of the blocking chain, not a victim. A single long-running or uncommitted transaction (a user who left a modal open, an app that never committed) blocks everyone behind it; the fix is that transaction, not killing sessions at random. For repeating deadlocks, capture the deadlock graph (extended events / system_health) and read the actual resource and lock order — the durable fix is usually an index or an access-order change in the *application*, which is the vendor's call. Escalate when: the head blocker is a vendor app's own process or the deadlock is inside vendor code — package the graph for the vendor.
   2. **Missing / bloated indexes & stale statistics** — a plan showing large scans or a huge estimated-vs-actual row skew points at missing indexes or stale stats. `UPDATE STATISTICS` (or a stats rebuild) is low-risk and often the fastest win; new indexes are a schema change with write-side cost and, on a vendor database, frequently unsupported (guardrail). Treat the "missing index" DMV hints as candidates to evaluate, never as a script to bulk-apply.
   3. **tempdb contention** — waits on PAGELATCH_* against tempdb allocation pages, or tempdb full. Check tempdb file count/sizing and autogrowth, and what is spilling to tempdb (sorts/hashes from a bad plan, version store from long transactions, an over-large query). Fixing the offending query often relieves tempdb more than resizing it.
   4. **Plan cache / parameter sniffing** — a query that is fast sometimes and slow other times for different inputs is the classic tell. Confirm from the cached plan vs the actual; the honest fixes (OPTION RECOMPILE, plan guides, query changes) touch how the app runs and belong with the app owner/vendor. A blanket `DBCC FREEPROCCACHE` on a production instance is a blunt, disruptive instrument — do not run it as a first move.
   5. **Resource pressure (CPU / memory / IO)** — high signal waits (SOS_SCHEDULER_YIELD, RESOURCE_SEMAPHORE, PAGEIOLATCH_*) plus host metrics. Check Max Server Memory is set (an unbounded instance starves the OS), the host isn't swapping, storage latency is sane, and no runaway job/backup is colliding with the workday. Escalate when: it's a host/storage sizing problem — that's an infrastructure decision, not a query fix.
6. **Verify and note.** Prove it with the same measurement that found it — the slow report now returns in time, the blocking chain is gone, the wait profile shifted. Plain-text note: version/edition, scope, the measured bottleneck (waits/plan), branch, action taken or handed off, and verification.

## Guardrails

- **Vendor-application database caution is the headline rule.** Many SQL databases back a line-of-business app (accounting, ERP, EHR, practice-management). On those, adding indexes, changing statistics settings, editing queries, or altering schema can break vendor support and the app itself. Read freely; before any *write* to a vendor DB, get the vendor's blessing or the client's explicit acceptance — pair with the vendor's own troubleshooting playbook and erp-generic-troubleshooting / lob-database-locks.
- No remote or T-SQL execution from here — all DMV queries and changes are guidance for a DBA/tech with proper access; use the RMM deep link (get_ninjaone_device_link) to hand a tech onto the host when that integration is enabled.
- Never run `DBCC FREEPROCCACHE`, kill sessions, shrink files, or drop/create indexes on production as a reflex — each is disruptive or destructive; state the impact and get sign-off, and prefer scheduling disruptive steps off-hours.
- Diagnose from wait stats and the actual plan; do not invent index recommendations, DMV syntax, or error meanings — web_search the instance's version and cite.
- Backups and recovery model belong to a separate concern — pair with the SQL backup/maintenance playbook rather than fixing log growth by breaking the log chain.
- Docs and Liongard coverage vary per tenant — note what you could not check and the dataprint age of anything you read. Notes destined for a PSA sync: plain text, no markdown or emojis.
