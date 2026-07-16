---
name: Liongard Datto RMM Read
description: Interrogate a client's Datto RMM footprint through the Liongard Datto RMM inspector — managed device inventory, agent/online status, patch state, monitored alerts, sites. Use for "what devices does RMM manage for them?", agent-coverage gaps, or patch-currency checks during triage.
category: Liongard Inspectors
tools: [liongard_environment, liongard_launchpoint, liongard_metric, liongard_query, liongard_detection, liongard_timeline, search_tickets, search_itglue, search_hudu, add_ticket_note]
---

# Liongard Datto RMM Read

Reads Datto RMM state — managed device inventory, agent online/offline status, patch state, monitored alerts, and site structure — from the Liongard Datto RMM inspector's dataprint, so a tech can judge RMM coverage without logging into the RMM console.

## When to use

- "What devices does Datto RMM manage at <client>?" / coverage-gap check.
- "Which agents are offline / not checking in?"
- "Are their machines patched?" — patch-currency question during triage.
- Reconciling RMM device count against AD/security-tool counts to find unmanaged machines.
- Post-change: "did monitoring or patch policy change on the RMM side?"

## What the inspector captures

Managed device inventory (with OS, last-seen, online status), agent/monitoring state, patch status, monitored alerts, and site/organization mapping. Exact coverage varies by inspector version.

## Steps

1. Follow liongard-inspectors/liongard-access-pattern: resolve the environment, `liongard_launchpoint` filtered by systemType "Datto RMM" / "Datto", verify last-run success, note dataprint age.
2. Pick the angle and query via `liongard_metric` or `liongard_query`. Illustrative JMESPath angles — verify field paths against the live dataprint; schemas vary by inspector version:
   - Inventory: device list with OS and last-seen — group by site.
   - Offline agents: devices whose last-check-in exceeds a threshold — list for follow-up.
   - Patch state: devices with missing/failed patches — flag laggards.
   - Alerts: open monitored alerts with severity.
3. For "what changed" questions, pull `liongard_detection` / `liongard_timeline` scoped to Datto RMM: device adds/removes, monitor/policy changes, agent-version shifts — line them up against the incident window.
4. Sanity-check surprising values (zero devices) — usually a wrong field path or partial inspection. Re-probe broadly.
5. Output: the requested facts in a compact table, source + dataprint age line, and flags (offline agents, patch laggards, coverage gaps). Offer a plain-text `add_ticket_note`.

## How this feeds tickets

- **Triage**: attach device/agent facts to a performance or availability ticket.
- **Coverage reconciliation**: RMM device count vs AD/AV counts surfaces unmanaged machines — label the delta, verify before asserting.
- **QBR evidence**: managed-device count, agent-health %, and patch-currency are recurring QBR line items — export with their as-of date.

## Guardrails

- Read-only: this skill never runs scripts, deploys software, or changes RMM policy. Remediation is a tech's job in the RMM console.
- Absence of a device in RMM is a *coverage lead*, not proof it is unmanaged — verify before asserting.
- Always state dataprint age; agent status changes constantly — an old dataprint on an active incident deserves a "verify live" caveat.
- If no Datto RMM launchpoint exists, degrade per the access pattern: docs → ticket history → "verify in console".
- Plain-text notes only.
