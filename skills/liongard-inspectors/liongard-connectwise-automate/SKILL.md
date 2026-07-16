---
name: Liongard ConnectWise Automate Read
description: Interrogate a client's ConnectWise Automate (LabTech) footprint through the Liongard Automate inspector — managed computer inventory, agent check-in status, patch state, monitors, locations. Use for "what does Automate manage for them?", offline-agent checks, or patch-currency questions during triage.
category: Liongard Inspectors
tools: [liongard_environment, liongard_launchpoint, liongard_metric, liongard_query, liongard_detection, liongard_timeline, search_tickets, search_itglue, search_hudu, add_ticket_note]
---

# Liongard ConnectWise Automate Read

Reads ConnectWise Automate (formerly LabTech) state — managed computers, agent check-in status, patch state, monitors, and location structure — from the Liongard Automate inspector's dataprint, so a tech can judge RMM coverage without logging into the Automate console.

## When to use

- "What computers does Automate manage at <client>?" / coverage-gap check.
- "Which agents haven't checked in?"
- "Are machines patched?" — patch-currency question during triage.
- Reconciling Automate agent count against AD/security-tool counts to find unmanaged machines.
- Post-change: "did a monitor or patch policy change on the Automate side?"

## What the inspector captures

Managed computer inventory (OS, last-contact, online status), agent state, patch status, monitor/alert state, and client/location mapping. Exact coverage varies by inspector version.

## Steps

1. Follow liongard-inspectors/liongard-access-pattern: resolve the environment, `liongard_launchpoint` filtered by systemType "Automate" / "ConnectWise Automate" / "LabTech", verify last-run success, note dataprint age.
2. Pick the angle and query via `liongard_metric` or `liongard_query`. Illustrative JMESPath angles — verify field paths against the live dataprint; schemas vary by inspector version:
   - Inventory: computer list with OS and last-contact — group by location.
   - Offline agents: computers whose last-contact exceeds a threshold.
   - Patch state: machines with missing/failed patches.
   - Monitors: monitors in a failed/alerting state.
3. For "what changed" questions, pull `liongard_detection` / `liongard_timeline` scoped to Automate: agent adds/removes, monitor/policy changes — line them up against the incident window.
4. Sanity-check surprising values (zero computers) — usually a wrong field path or partial inspection. Re-probe broadly.
5. Output: the requested facts in a compact table, source + dataprint age line, and flags (offline agents, patch laggards, coverage gaps). Offer a plain-text `add_ticket_note`.

## How this feeds tickets

- **Triage**: attach computer/agent facts to a performance or availability ticket.
- **Coverage reconciliation**: Automate agent count vs AD/AV counts surfaces unmanaged machines — label the delta, verify before asserting.
- **QBR evidence**: managed-count, agent-health %, and patch-currency are recurring QBR line items — export with their as-of date.

## Guardrails

- Read-only: this skill never runs scripts, deploys software, or changes Automate policy. Remediation is a tech's job in the Automate console.
- Absence of an agent in Automate is a *coverage lead*, not proof a machine is unmanaged — verify before asserting.
- Always state dataprint age; agent status changes constantly — an old dataprint on an active incident deserves a "verify live" caveat.
- If no Automate launchpoint exists, degrade per the access pattern: docs → ticket history → "verify in console".
- Plain-text notes only.
