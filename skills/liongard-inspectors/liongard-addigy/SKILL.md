---
name: Liongard Addigy Read
description: Interrogate a client's Addigy Apple/macOS MDM through the Liongard Addigy inspector — enrolled Mac inventory, OS versions, policy assignment, MDM/agent check-in, FileVault and compliance state. Use for "what Macs does Addigy manage?", macOS patch-currency, or Apple-fleet compliance checks during triage.
category: Liongard Inspectors
tools: [liongard_environment, liongard_launchpoint, liongard_metric, liongard_query, liongard_detection, liongard_timeline, search_tickets, search_itglue, search_hudu, add_ticket_note]
---

# Liongard Addigy Read

Reads Addigy MDM state — enrolled Macs, OS versions, policy assignments, agent/MDM check-in, and compliance signals (FileVault, etc.) — from the Liongard Addigy inspector's dataprint, so a tech can judge the Apple-fleet posture without logging into the Addigy console.

## When to use

- "What Macs does Addigy manage at <client>?" / Apple-fleet coverage check.
- "Which Macs are behind on macOS?" — OS-currency question during triage.
- "Is FileVault on across their Macs?" — encryption/compliance check.
- "Which Macs haven't checked in?"
- Post-change: "did an Addigy policy assignment change?"

## What the inspector captures

Enrolled Mac inventory (model, macOS version, last-check-in), policy/group assignment, agent and MDM enrollment state, and compliance signals such as FileVault status. Exact coverage varies by inspector version.

## Steps

1. Follow liongard-inspectors/liongard-access-pattern: resolve the environment, `liongard_launchpoint` filtered by systemType "Addigy", verify last-run success, note dataprint age.
2. Pick the angle and query via `liongard_metric` or `liongard_query`. Illustrative JMESPath angles — verify field paths against the live dataprint; schemas vary by inspector version:
   - Inventory: Mac list with model and macOS version — flag versions behind current.
   - Check-in: Macs whose last-check-in exceeds a threshold.
   - Encryption: Macs with FileVault disabled.
   - Policy: policy/group assignment per device.
3. For "what changed" questions, pull `liongard_detection` / `liongard_timeline` scoped to Addigy: policy changes, enrollment adds/removes — line them up against the incident window.
4. Sanity-check surprising values (zero Macs) — usually a wrong field path or partial inspection. Re-probe broadly.
5. Output: the requested facts in a compact table, source + dataprint age line, and flags (macOS laggards, FileVault-off, stale check-ins). Offer a plain-text `add_ticket_note`.

## How this feeds tickets

- **Triage**: attach the Mac fleet picture to an Apple endpoint ticket.
- **Compliance evidence**: FileVault adoption and macOS currency are recurring audit/QBR line items — export with their as-of date.
- **Coverage reconciliation**: Addigy Mac count vs AD/directory counts surfaces unmanaged Apple devices — label the delta, verify before asserting.

## Guardrails

- Read-only: this skill never changes Addigy policy or enrollment. Remediation is a tech's job in the console.
- Absence of a Mac in Addigy is a *coverage lead*, not proof it is unmanaged — verify before asserting.
- Always state dataprint age; fleet state changes — an old dataprint on an active incident deserves a "verify live" caveat.
- If no Addigy launchpoint exists, degrade per the access pattern: docs → ticket history → "verify in console".
- Plain-text notes only.
