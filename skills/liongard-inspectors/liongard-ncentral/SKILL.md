---
name: Liongard N-central Read
description: Interrogate a client's N-able N-central footprint through the Liongard N-central inspector — managed device inventory, agent/probe status, patch state, monitored services, customers/sites. Use for "what does N-central manage for them?", offline-agent checks, or patch-currency questions during triage.
category: Liongard Inspectors
tools: [liongard_environment, liongard_launchpoint, liongard_metric, liongard_query, liongard_detection, liongard_timeline, search_tickets, search_itglue, search_hudu, add_ticket_note]
---

# Liongard N-central Read

Reads N-able N-central state — managed devices, agent/probe status, patch state, monitored services, and customer/site structure — from the Liongard N-central inspector's dataprint, so a tech can judge RMM coverage without logging into the N-central console.

## When to use

- "What devices does N-central manage at <client>?" / coverage-gap check.
- "Which agents or probes are offline?"
- "Are machines patched?" — patch-currency question during triage.
- Reconciling N-central device count against AD/security-tool counts to find unmanaged machines.
- Post-change: "did a monitoring service or patch policy change on the N-central side?"

## What the inspector captures

Managed device inventory (OS, last-seen, status), agent/probe state, patch status, monitored-service state, and customer/site mapping. Exact coverage varies by inspector version.

## Steps

1. Follow liongard-inspectors/liongard-access-pattern: resolve the environment, `liongard_launchpoint` filtered by systemType "N-central" / "N-able", verify last-run success, note dataprint age.
2. Pick the angle and query via `liongard_metric` or `liongard_query`. Illustrative JMESPath angles — verify field paths against the live dataprint; schemas vary by inspector version:
   - Inventory: device list with OS and last-seen — group by customer/site.
   - Offline agents: devices whose last-check-in exceeds a threshold.
   - Patch state: machines with missing/failed patches.
   - Services: monitored services in a failed/warning state.
3. For "what changed" questions, pull `liongard_detection` / `liongard_timeline` scoped to N-central: device adds/removes, service/policy changes — line them up against the incident window.
4. Sanity-check surprising values (zero devices) — usually a wrong field path or partial inspection. Re-probe broadly.
5. Output: the requested facts in a compact table, source + dataprint age line, and flags (offline agents, patch laggards, coverage gaps). Offer a plain-text `add_ticket_note`.

## How this feeds tickets

- **Triage**: attach device/agent facts to a performance or availability ticket.
- **Coverage reconciliation**: N-central device count vs AD/AV counts surfaces unmanaged machines — label the delta, verify before asserting.
- **QBR evidence**: managed-count, agent-health %, and patch-currency are recurring QBR line items — export with their as-of date.

## Guardrails

- Read-only: this skill never runs scripts, deploys software, or changes N-central policy. Remediation is a tech's job in the console.
- Absence of a device in N-central is a *coverage lead*, not proof it is unmanaged — verify before asserting.
- Always state dataprint age; agent status changes constantly — an old dataprint on an active incident deserves a "verify live" caveat.
- If no N-central launchpoint exists, degrade per the access pattern: docs → ticket history → "verify in console".
- Plain-text notes only.
