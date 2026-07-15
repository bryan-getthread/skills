---
name: Liongard SQL Server Read
description: Interrogate a client's SQL Server instances through Liongard — version/SP/CU posture, database inventory and sizes, backup recency per database, sysadmin role members. Use for "when did that database last back up", version-currency checks, or sysadmin audits during triage and reviews.
category: Liongard Inspectors
tools: [liongard_environment, liongard_launchpoint, liongard_metric, liongard_query, liongard_detection, liongard_timeline, search_tickets, search_itglue, search_hudu, add_ticket_note]
---

# Liongard SQL Server Read

Reads SQL Server instance state — version/patch level, databases, backup history, security principals — from the Liongard Microsoft SQL Server inspector. The reads that save incidents: per-database backup recency (native backups often diverge from the image-backup story) and who holds sysadmin.

## When to use

- LOB-app ticket touches a database: "which instance/database, how big, when did it last back up?"
- "What SQL versions are across <client>'s servers?" — currency/EOL sweep (old SQL versions linger for years behind LOB apps).
- "Who has sysadmin on that instance?" — access audit, or before blaming permissions in an app incident.
- Pre-maintenance check: backup recency before anyone touches the instance.
- "Did anything change on the SQL server?" — new database, login change, config change.

## What the inspector captures

Instance identity, SQL version/edition/service pack/CU, database inventory (name, size, state, recovery model where present), backup history per database (last full/diff/log), server principals and role membership (sysadmin especially), and instance configuration. Coverage varies by SQL version and inspector version.

## Steps

1. Follow liongard-inspectors/liongard-access-pattern: resolve environment, `liongard_launchpoint` with systemType "SQL Server" (try "Microsoft SQL"), verify last-run success, note dataprint age. Multiple instances per host exist — enumerate launchpoints and name the instance in every answer.
2. Query via `liongard_metric` or `liongard_query`. Illustrative JMESPath angles — verify field paths against the live dataprint; schemas vary by inspector version:
   - Version posture: version/edition/SP/CU per instance — report verbatim; flag versions that look out of mainstream/extended support with "verify against Microsoft's SQL lifecycle" rather than asserting from memory.
   - Database inventory: name, size, state — offline/suspect databases are headline findings.
   - **Backup recency (the priority read)**: last full backup date per database — flag databases with no full backup in 7 days (tighten to 24–48h for LOB/production databases), and databases in FULL recovery with no recent log backup (the runaway-log-file setup).
   - Sysadmin: sysadmin role members — flag beyond the expected sa + DBA/service set; disabled 'sa' is good hygiene worth confirming.
3. Reconcile the backup story: native SQL backups vs the image/agent backup platform can both exist — if the client runs Veeam, cross-check liongard-inspectors/liongard-veeam-posture (application-aware processing may be the real backup). Present both and say which is protecting the databases per the evidence.
4. "What changed": `liongard_detection` / `liongard_timeline` — new databases, principal/role changes, config changes ordered against the incident window. A new sysadmin login without a matching ticket is a flag on its own.
5. Output: per-instance tables (version, databases, backup recency, sysadmin), flags with severity, source + dataprint age. Offer a plain-text `add_ticket_note`.

## How this feeds tickets

- **Triage**: LOB-app tickets open with instance/database/backup facts; "is it safe to restart the SQL box" gets a backup-recency answer first.
- **Incident correlation**: principal or config changes vs app-incident window (time-adjacency, bounded by inspector cadence).
- **QBR / risk**: SQL version currency, unsupported versions behind LOB apps, and backup-gap findings are strong QBR and risk-review lines.

## Guardrails

- Read-only: never run SQL, change principals, or trigger backups.
- Backup-recency findings state the dataprint age explicitly — "no full backup in 9 days as of <timestamp>" — and defer "backups are healthy" claims to restore tests.
- A "missing native backup" is only a gap if nothing else protects the database — do the Veeam/image cross-check before flagging, or phrase as "no native SQL backup found; confirm image-level protection covers it".
- Sysadmin findings are "unrecognized — verify" inputs, not accusations; LOB installers often create legitimate-looking service logins.
- Version/EOL claims defer lifecycle dates to Microsoft's published lifecycle.
- Degrade per the access pattern (docs → ticket history) when the inspector is absent — SQL instances hiding inside LOB servers often have no inspector; recommend adding one when the client's tickets keep touching that database.
- Plain-text notes only.
