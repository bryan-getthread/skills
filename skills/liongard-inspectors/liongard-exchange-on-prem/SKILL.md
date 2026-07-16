---
name: Liongard Exchange On-Prem Read
description: Interrogate a client's on-premises Microsoft Exchange through the Liongard Exchange inspector — server version/CU/build, databases, mailbox inventory, connectors, and admin access. Use for "what's their Exchange config?", CU/patch-currency checks, database/mailbox inventory, or mail-flow triage context.
category: Liongard Inspectors
tools: [liongard_environment, liongard_launchpoint, liongard_metric, liongard_query, liongard_detection, liongard_timeline, search_tickets, search_itglue, search_hudu, add_ticket_note]
---

# Liongard Exchange On-Prem Read

Reads on-premises Microsoft Exchange state — server version/CU/build, mailbox databases, mailbox inventory, send/receive connectors, and admin access — from the Liongard Exchange inspector's dataprint, so a tech gets the mail-server picture during triage without an EMS/ECP session. (For Exchange Online / M365, use liongard-m365-tenant.)

## When to use

- "What CU/build is <client>'s Exchange server on?" — currency/vulnerability check (Exchange CVEs are commonly build-gated).
- "What mailbox databases exist and how big are they?" — capacity/health context.
- "How many mailboxes / which are on which database?" — inventory.
- "What send/receive connectors and mail-flow routing are configured?" — mail-flow triage.
- Post-change: "did a connector or database setting change?"

## What the inspector captures

Exchange server(s) with version/CU/build, mailbox databases (size, mount status), mailbox inventory, send/receive connectors, and administrative access/roles. Exact coverage varies by inspector version.

## Steps

1. Follow liongard-inspectors/liongard-access-pattern: resolve the environment, `liongard_launchpoint` filtered by systemType "Exchange" / "Exchange Server", verify last-run success, note dataprint age. Confirm it is the on-prem inspector, not Exchange Online.
2. Pick the angle and query via `liongard_metric` or `liongard_query`. Illustrative JMESPath angles — verify field paths against the live dataprint; schemas vary by inspector version:
   - Version: server version/CU/build — compare against Microsoft's current CU and known-vuln advisories.
   - Databases: database list with size and mount status — flag large/dismounted.
   - Mailboxes: mailbox count per database.
   - Connectors: send/receive connector list with smart-host/routing.
   - Admins: Exchange admin role assignments.
3. For "what changed" questions, pull `liongard_detection` / `liongard_timeline` scoped to Exchange: CU/build upgrades, connector changes, database adds/removes, admin changes — line them up against the incident window.
4. Sanity-check surprising values (zero databases) — usually a wrong field path or partial inspection. Re-probe broadly.
5. Output: the requested facts in a compact table, source + dataprint age line, and flags (outdated CU/build, dismounted databases, unexpected connectors, excess admins). Offer a plain-text `add_ticket_note`.

## How this feeds tickets

- **Triage**: attach the version/connector picture to a mail-flow ticket so the tech starts with the routing.
- **Currency review**: Exchange CU/build currency is a high-priority security finding (on-prem Exchange has repeatedly been actively exploited) — surface with the as-of date, recommend the tech confirm live and patch.
- **Incident correlation**: a mail-flow issue after a detected connector or CU change is a lead — label it time-adjacency, not proven cause.

## Guardrails

- Read-only: this skill never changes Exchange config, connectors, or mailboxes. Remediation is a tech's job via EMS/ECP.
- Never paste connector credentials or smart-host secrets into notes even if the dataprint exposes them — reference "credentials on file in <doc platform>".
- Always state dataprint age; mail config changes — an old dataprint on an active mail-flow incident deserves a "verify live" caveat.
- Do not confuse on-prem Exchange with Exchange Online — if the question is about M365-hosted mail, use liongard-m365-tenant.
- If no on-prem Exchange launchpoint exists, degrade per the access pattern: docs → ticket history → "verify on server".
- Plain-text notes only.
