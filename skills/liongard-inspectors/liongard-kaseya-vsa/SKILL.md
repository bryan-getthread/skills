---
name: Liongard Kaseya VSA Read
description: Interrogate a client's Kaseya VSA footprint through the Liongard Kaseya VSA inspector — managed agent inventory, online/check-in status, patch state, monitor sets, organizations/machine groups. Use for "what does VSA manage for them?", offline-agent checks, or patch-currency questions during triage.
category: Liongard Inspectors
tools: [liongard_environment, liongard_launchpoint, liongard_metric, liongard_query, liongard_detection, liongard_timeline, search_tickets, search_itglue, search_hudu, add_ticket_note]
---

# Liongard Kaseya VSA Read

Reads Kaseya VSA state — managed agents, online/check-in status, patch state, monitor sets, and organization/machine-group structure — from the Liongard Kaseya VSA inspector's dataprint, so a tech can judge RMM coverage without logging into the VSA console.

## When to use

- "What machines does Kaseya VSA manage at <client>?" / coverage-gap check.
- "Which agents haven't checked in?"
- "Are machines patched?" — patch-currency question during triage.
- Reconciling VSA agent count against AD/security-tool counts to find unmanaged machines.
- Post-change: "did a monitor set or patch policy change on the VSA side?"

## What the inspector captures

Managed agent inventory (OS, last-check-in, online status), agent state, patch status, monitor-set state, and organization/machine-group mapping. Exact coverage varies by inspector version.

## Steps

1. Follow liongard-inspectors/liongard-access-pattern: resolve the environment, `liongard_launchpoint` filtered by systemType "Kaseya VSA" / "Kaseya" / "VSA", verify last-run success, note dataprint age.
2. Pick the angle and query via `liongard_metric` or `liongard_query`. Illustrative JMESPath angles — verify field paths against the live dataprint; schemas vary by inspector version:
   - Inventory: agent list with OS and last-check-in — group by machine group.
   - Offline agents: agents whose last-check-in exceeds a threshold.
   - Patch state: machines with missing/failed patches.
   - Monitors: monitor sets in an alarm state.
3. For "what changed" questions, pull `liongard_detection` / `liongard_timeline` scoped to VSA: agent adds/removes, monitor/policy changes — line them up against the incident window.
4. Sanity-check surprising values (zero agents) — usually a wrong field path or partial inspection. Re-probe broadly.
5. Output: the requested facts in a compact table, source + dataprint age line, and flags (offline agents, patch laggards, coverage gaps). Offer a plain-text `add_ticket_note`.

## How this feeds tickets

- **Triage**: attach agent facts to a performance or availability ticket.
- **Coverage reconciliation**: VSA agent count vs AD/AV counts surfaces unmanaged machines — label the delta, verify before asserting.
- **QBR evidence**: managed-count, agent-health %, and patch-currency are recurring QBR line items — export with their as-of date.

## Guardrails

- Read-only: this skill never runs scripts, deploys software, or changes VSA policy. Remediation is a tech's job in the console.
- Absence of an agent in VSA is a *coverage lead*, not proof a machine is unmanaged — verify before asserting.
- Always state dataprint age; agent status changes constantly — an old dataprint on an active incident deserves a "verify live" caveat.
- If no Kaseya VSA launchpoint exists, degrade per the access pattern: docs → ticket history → "verify in console".
- Plain-text notes only.
