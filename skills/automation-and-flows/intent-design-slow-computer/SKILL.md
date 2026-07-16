---
name: Slow Computer Intent Design
description: Build the "my computer is slow" intent — run the user through the top quick self-help checks first, and if unresolved capture diagnostics so the ticket arrives triage-ready. Use when asked to "build a slow computer intent", "deflect performance complaints", or when slow-PC tickets show up in Intent Mining.
category: Automation & Flows
tools: [list_intents, get_intent, create_intent, update_intent, set_variation_arguments, set_variation_replies, update_variation, search_tickets, search_knowledge_base]
---

# Slow Computer Intent Design

Guide the admin through building an intent for "my computer is slow" — a high-volume, low-specificity complaint. The intent walks the user through the handful of quick checks that resolve most cases, and where they don't, captures the diagnostics a technician always ends up asking for, so the ticket lands triage-ready instead of as a one-line "it's slow".

## When to use

- "Build an intent for slow-computer complaints."
- "'My PC is slow' tickets come in with no detail — guide them and capture diagnostics."
- Intent Mining flagged performance / slowness as a high-volume topic.

## Design

**Trigger phrases (adapt to real ticket language):** "my computer is slow", "PC running slow", "laptop is really slow", "everything is lagging", "taking forever to open apps", "computer keeps freezing", "slow performance", "my machine is sluggish".
Near-miss watch: "the internet/wifi is slow" is a network issue (route to wifi/troubleshooting); "a specific app is slow/broken" is an application issue — keep triggers scoped to general device slowness.

**Arguments to collect (paired with the quick checks):**
- Which device (work laptop/desktop; asset tag if known).
- When it started and whether it's constant or intermittent (drives severity).
- Is it one app or everything; does it happen after boot, over the day, or during specific tasks.
- Whether they've rebooted recently (uptime is the single most common cause).

**Reply flow (guided self-help first):**
1. Walk the top quick checks in order, one manageable step at a time: reboot if uptime is long; check for a pending Windows/app update; close heavy background apps / check what's using resources; confirm free disk space. Use the client's KB steps where they exist.
2. After each, ask if it helped. If resolved → confirm and close as deflected.
3. If the quick checks don't resolve it → create a ticket with the collected diagnostics (device, onset, scope, reboot/update state, and what the user observed) and route to the endpoint/desktop-support workflow. If an RMM/remote-monitoring tool is available to the tech, note it in the ticket rather than trying to read metrics from within the intent.

**Handoff rule:** the intent guides safe, user-runnable checks only (reboot, updates, closing apps, disk space) — it never has the user edit the registry, uninstall system components, or run admin-level fixes. Deeper diagnosis and any remediation are technician/workflow actions.

**Variation hooks (per client):** the client's specific quick-check KB, whether an RMM tool covers these endpoints (informs the ticket, not a user step), the endpoint-support routing target, any managed-update cadence to mention.

**Success metric:** deflection rate — slowness conversations resolved by the quick checks without a ticket (reboot/update alone clears a meaningful share). Secondary: first-touch completeness on escalations, since the captured diagnostics should remove the tech's usual first round of questions.

## Steps

1. `list_intents` — check for an existing performance/troubleshooting intent; prefer `update_intent` over a duplicate.
2. `search_tickets` for past slow-computer tickets; note which quick checks resolved them and which diagnostics techs always had to ask for (those become the guided steps and arguments).
3. `search_knowledge_base` for the client's performance-troubleshooting guide; reference its steps rather than restating ones that may drift.
4. Draft the full spec: triggers, arguments, the ordered quick-check ladder, escalation with diagnostics, routing target, variations, and a test plan (5 should-match, 3–5 should-not including a slow-internet and an app-specific near-miss). Show before any write.
5. On explicit confirmation: `create_intent`, then `set_variation_arguments` / `set_variation_replies` / `update_variation`.
6. Report what was created, restate the test plan, recommend activation only after tests pass. Do not activate.

## Guardrails

- Intent tools are admin-only; if absent, degrade to a complete written spec for an admin to apply.
- Guide only safe, user-runnable checks (reboot, updates, closing apps, disk space); never walk a user through registry edits, uninstalling system components, or admin-level fixes.
- Don't overload the user: one step at a time, confirming after each — a wall of troubleshooting is a worse experience than a ticket.
- The intent does not read RMM metrics or remediate; note tool availability in the ticket for the tech instead. Do not invent the client's KB or RMM coverage — placeholder unknowns and flag before activation.
- Confirm before any write; never activate on the member's behalf. Ticket field block in plain text (PSA-safe, no markdown).
