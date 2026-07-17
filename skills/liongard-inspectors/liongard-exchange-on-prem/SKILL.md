---
name: Liongard Exchange On-Prem Read
description: Interrogate a client's on-premises Microsoft Exchange through the Liongard Exchange inspector — server version/CU/build, databases, mailbox inventory, connectors, and admin access. Use for "what's their Exchange config?", CU/patch-currency checks, database/mailbox inventory, or mail-flow triage context.
category: Liongard Inspectors
tools: [liongard_environment, liongard_launchpoint, liongard_metric, liongard_query, liongard_detection, liongard_timeline]
connectors: [Liongard]
---

# Liongard Exchange On-Prem Read

**When to use:** "What CU/build is <client>'s Exchange server on?" (Exchange CVEs are commonly build-gated), "what mailbox databases exist and how big are they?", mailbox inventory, "what send/receive connectors and mail-flow routing are configured?", or "did a connector or database setting change?". For Exchange Online, use the M365 tenant read.

## Prompt

```
Read on-premises Microsoft Exchange state for CLIENT_NAME from the Liongard Exchange inspector. Read-only — Exchange config, connectors, and mailboxes are a technician's job via EMS/ECP.

1. Resolve the environment (liongard_environment), then liongard_launchpoint filtered by systemType "Exchange" / "Exchange Server". Confirm it is the on-prem inspector, not Exchange Online. Verify last-run success and note the dataprint age — carry "as of <timestamp>." Mail config changes, so an old dataprint on an active mail-flow incident deserves a "verify live" caveat.
2. Pick the angle and query via liongard_metric or liongard_query. Verify every JMESPath field path against the live dataprint (schemas vary by inspector version):
   - Version: server version/CU/build — report verbatim; compare against Microsoft's current CU and known-vuln advisories rather than asserting from memory.
   - Databases: database list with size and mount status — flag large/dismounted.
   - Mailboxes: mailbox count per database.
   - Connectors: send/receive connector list with smart-host/routing.
   - Admins: Exchange admin role assignments.
3. "What changed?" → liongard_detection / liongard_timeline scoped to Exchange: CU/build upgrades, connector changes, database adds/removes, admin changes — ordered against the incident window (time-adjacency, not proven cause).
4. Sanity-check surprising zeros (zero databases) — usually a wrong field path or partial inspection; re-probe broadly. Never paste connector credentials or smart-host secrets into notes — reference "credentials on file in <doc platform>." Exchange CU/build currency is a high-priority security finding (on-prem Exchange has repeatedly been actively exploited) — surface with the as-of date and recommend the tech patch.
5. Output: a compact table, source + dataprint age line, flags (outdated CU/build, dismounted databases, unexpected connectors, excess admins). Offer a plain-text add_ticket_note. Degradation: no on-prem Exchange launchpoint → docs (search_itglue/search_hudu) → ticket history (search_tickets) → "verify on server."
```
