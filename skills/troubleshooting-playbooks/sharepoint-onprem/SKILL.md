---
name: SharePoint On-Prem
description: Diagnose on-premises SharePoint Server problems — search crawl failures and stale results, content-database health and mounting, and permission-inheritance confusion — from ULS logs, crawl-log evidence, and the actual permission chain, never by breaking inheritance blindly. For OneDrive/SharePoint Online sync use onedrive-sharepoint-sync.
category: Troubleshooting Playbooks
tools: [search_tickets, search_knowledge_base, search_itglue, search_hudu, add_ticket_note, web_search]
---

# SharePoint On-Prem

On-prem SharePoint concentrates its failures in three places: search (crawls that stop or return stale/no results), content databases (health, mounting, and the config-DB relationship), and permissions (inheritance broken somewhere up the chain). This playbook reads the ULS and crawl logs before acting — and treats permission inheritance as something to trace, not to shatter. SharePoint Online / OneDrive sync is a different animal — use onedrive-sharepoint-sync.

## When to use

- Search returns stale, missing, or no results; a crawl is stuck, failing, or hasn't completed
- A site/content database won't mount, shows unhealthy, or a site collection is inaccessible
- Users can't access a site/library/item they should — or can access one they shouldn't
- After a patch/upgrade: sites broken, "the farm needs configuration", or a service app down

## Steps

1. **Version and farm topology first.** search_itglue / search_hudu / search_knowledge_base for the farm: SharePoint Server version and build (a partially-patched farm — binaries updated but the config wizard not run — is a classic broken state), the server roles (WFE/app/search), the SQL back end and content-DB layout, the search topology, and the web-app/authentication model (classic vs claims, NTLM/Kerberos). Note the patch level. If a Liongard Windows/SQL inspector runs, corroborate service/DB state via liongard_launchpoint / liongard_metric and note dataprint age.
2. **History first.** search_tickets for this client + SharePoint: a recent cumulative-update/patch (and whether PSConfig ran after), a SQL change, a content-DB move, or a permissions change. Broad breakage right after a patch usually means the config wizard hasn't finished the upgrade.
3. **Get the log evidence before theorizing.** For search: the **crawl log** (per-item errors and the crawl's last-completed state) and the search service application health. For farm/site errors: the **ULS logs** filtered by the correlation ID SharePoint shows on the error page (that correlation ID is the key that turns a generic error into a specific one). For content DBs: the DB's status in Central Admin and SQL. Read the correlation-ID'd ULS entry — don't act on the yellow error page alone.
4. **Branch:**
   1. **Search crawl failures / stale results** — results are old or missing: read the crawl log. A crawl stopped/paused, the crawl account lost access (permissions or a password change), the content source or start address is wrong, or the index is corrupt. Stale results usually mean the crawl isn't completing — fix the crawl (account, connectivity) and let it complete; a full crawl rebuilds after an index problem. Escalate when: the search index/topology itself is corrupt — resetting an index is a rebuild-cost decision.
   2. **Content database / site access** — a content DB won't mount or a site collection is down: check the DB status in Central Admin and its SQL health, whether the DB is at the right schema version (a version mismatch after a partial patch blocks mount), and free space. Mounting/dismounting content DBs and any DB-level action is coordinated with the SQL owner. Never detach or alter a content DB in SQL directly — SharePoint owns that relationship; do it through SharePoint's tools.
   3. **Permission inheritance** — a user can't access (or wrongly can): trace the chain — site → library → folder → item — and find where inheritance is broken and unique permissions were set. The problem is almost always a broken-inheritance point granting/denying differently than the parent, or a SharePoint group's membership. Fix at the right level; **do not break inheritance further** to patch one item (unique permissions at scale are unmanageable and slow). Restoring inheritance is often the real fix. Pair with file-share-permissions thinking but SharePoint's model is groups + inheritance, not NTFS.
   4. **Post-patch / farm config** — "farm needs configuring", services down, or sites erroring after an update: the CU installed the binaries but PSConfig (the upgrade step) didn't complete on all servers. Running the configuration wizard / `PSConfig` to finish the upgrade is the documented fix — but it's a farm change; coordinate a window and ensure a farm/DB backup exists first. Escalate the planning.
5. **Verify and note.** Success is concrete: search returns fresh results after a completed crawl, the site/content DB is healthy and accessible, or the user reaches exactly what they should (and not what they shouldn't). Plain-text note: version/build, correlation ID / crawl-log evidence, branch, action or handoff, verification.

## Guardrails

- No remote execution — Central Admin, PowerShell/PSConfig, and ULS steps are guidance for a tech with farm-admin access; use get_ninjaone_device_link to reach a farm server when the RMM integration is enabled.
- **Never touch SharePoint's databases directly in SQL** (editing, detaching, "fixing" a content DB) — it is unsupported and corrupts the farm; act through SharePoint's own tools and coordinate with the SQL owner.
- Don't break permission inheritance to solve one access request — trace to the broken-inheritance point and fix there; unique per-item permissions at scale are a mess. Restoring inheritance is frequently the correct fix.
- Running PSConfig / a CU upgrade step is a farm-level change — ensure a backup, plan a window, and coordinate; don't run it reactively under pressure.
- Search index resets and full crawls have a rebuild cost — set expectations before triggering.
- Do not invent PowerShell syntax, build-version behaviours, or ULS categories — web_search Microsoft's current on-prem docs and cite.
- Docs/Liongard coverage varies per tenant — note what you couldn't check and dataprint age. Notes destined for a PSA sync: plain text, no markdown or emojis.
