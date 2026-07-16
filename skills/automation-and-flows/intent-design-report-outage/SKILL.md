---
name: Report Outage Intent Design
description: Build the "is it down for everyone" intake intent — site and scope questions that turn a panicked outage report into a dispatch-ready incident ticket in under a minute. Use when asked to "build an outage intent", "handle 'everything is down' reports", or when outage reports surface in Intent Mining.
category: Automation & Flows
tools: [list_intents, get_intent, create_intent, update_intent, set_variation_arguments, set_variation_replies, update_variation, search_tickets]
---

# Report Outage Intent Design

Guide the admin through building an outage-report intent optimized for speed: a minimal scope-and-site question set that makes the resulting ticket dispatch-ready (what, where, how many, since when) without holding a possibly-major incident in conversation any longer than necessary.

## When to use

- "Build an intent for outage reports."
- "When something's down we get ten vague tickets and no scope info."
- Intent Mining shows clustered "server down / internet down / can't work" bursts.

## Design

**Trigger phrases (adapt to real ticket language):** "everything is down", "server is down", "internet is down for the office", "nobody can work", "system is down", "we're all offline", "the whole office lost connection", "is it down for everyone", "major outage", "can't access anything", "all our systems are offline".
Near-miss watch: "my computer is down" (one user) is a normal support ticket — a scope question rescues misfires; "<application> is slow" is degradation, still worth the intake but at lower urgency.

**Arguments to collect (minimal by design — speed beats completeness here):**
1. **What is down:** internet, a specific application/server, phones, everything (placeholder: <application>/<device>).
2. **Scope:** just you / your team / the whole site / multiple sites — the "is it down for everyone" question, asked explicitly.
3. **Site(s)** affected (<location>).
4. **Since when**, and did anything precede it (power blip, storm, maintenance notice).
5. A callback contact and number — assume the reporter's usual channels may be down.

**Reply flow:**
1. Ask only the five intake questions; accept partial answers — never block ticket creation on completeness during a suspected outage.
2. If scope = one user → reassure, convert to a normal support ticket at normal priority.
3. Otherwise create a high-priority ticket titled with what+where ("<application> down — <location>, multiple users"), body carrying the full intake in plain text; route to the client's incident/dispatch path.
4. Reply confirming the report is logged as a priority incident and that the desk will use the callback contact; do NOT speculate on cause or promise restoration times.
5. If more reports of the same outage arrive while it is ongoing, the design intent is one incident, not ten tickets — recommend the admin pair this with a desk-side flow that flags probable duplicates for merging (merge itself stays a human action).

**Handoff rule:** everything after intake is human. The intent never declares an outage over, never states cause, and never gives an ETA.

**Variation hooks (per client):** site list, critical systems worth naming in triggers (their LOB app going down is the outage that matters), incident routing/board, after-hours emergency instructions, status-page URL if the client has one.

**Success metric:** dispatch-readiness — percentage of outage tickets containing what/where/scope/since/callback at creation — and time-from-first-message-to-ticket (should be minutes). Duplicate-ticket count per incident trending down is the secondary win.

## Steps

1. `list_intents` — check for an existing outage/incident intent; prefer updating. Also note the office-wifi intent's triggers to avoid collision ("no internet in the office" could match either — agree the boundary with the admin: multi-user reports land here).
2. `search_tickets` for past outage bursts; mine the first messages users sent (triggers) and what dispatch had to ask afterwards (validates the five questions).
3. Draft the full spec: triggers, five-question intake, one-user demotion branch, priority routing, variations. Test plan: 5 should-match, 3–5 should-not (single-user-down and slow-app near-misses).
4. Show the draft; on explicit confirmation call `create_intent` then the variation tools.
5. Report what was created, restate the test plan, recommend activation after tests pass — and suggest a fire-drill test message before relying on it. Do not activate.

## Guardrails

- Intent tools are admin-only; if absent, output the complete written spec for an admin to apply.
- Speed over completeness: never let missing answers delay ticket creation for a multi-user report; file with what you have and note the gaps.
- Replies must never speculate on cause, blame a vendor, promise an ETA, or declare recovery — incident communication is a human job.
- Priority/urgency mapping comes from the client's real priority scheme; do not invent SLA language.
- Confirm before any write; never activate on the member's behalf. Intake block in plain text.
