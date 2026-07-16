---
name: Liongard 3CX Read
description: Interrogate a client's 3CX phone system through the Liongard 3CX inspector — version/build, extensions, trunks/SIP providers, admins, backup and security settings. Use for "what's their 3CX config?", version-currency checks, extension/trunk inventory, or telephony triage context. Read-only — no dialing or call control.
category: Liongard Inspectors
tools: [liongard_environment, liongard_launchpoint, liongard_metric, liongard_query, liongard_detection, liongard_timeline, search_tickets, search_itglue, search_hudu, add_ticket_note]
---

# Liongard 3CX Read

Reads 3CX phone-system state — version/build, extensions, SIP trunks/providers, admins, and backup/security settings — from the Liongard 3CX inspector's dataprint, so a tech gets the telephony configuration during triage without logging into the 3CX management console. This is configuration reading only — it does not dial, route, or control calls.

## When to use

- "What version/build is <client>'s 3CX on?" — currency/vulnerability check.
- "How many extensions are configured / which are unregistered?"
- "What SIP trunks/providers are set up?" — telephony troubleshooting context.
- "Is 3CX backup enabled?" / "Who has admin?" — resilience/security check.
- Post-change: "did a trunk, extension, or setting change on the 3CX side?"

## What the inspector captures

Version/build, extension inventory (with registration state), SIP trunks/providers, administrators, and backup/security settings. Exact coverage varies by inspector version.

## Steps

1. Follow liongard-inspectors/liongard-access-pattern: resolve the environment, `liongard_launchpoint` filtered by systemType "3CX", verify last-run success, note dataprint age.
2. Pick the angle and query via `liongard_metric` or `liongard_query`. Illustrative JMESPath angles — verify field paths against the live dataprint; schemas vary by inspector version:
   - Version: current version/build — compare against vendor's current release.
   - Extensions: extension list with registration status — flag unregistered.
   - Trunks: SIP trunk/provider list with status.
   - Backup/security: backup-enabled flag, admin list.
3. For "what changed" questions, pull `liongard_detection` / `liongard_timeline` scoped to 3CX: version upgrades, trunk/extension changes, admin changes — line them up against the incident window.
4. Sanity-check surprising values (zero extensions) — usually a wrong field path or partial inspection. Re-probe broadly.
5. Output: the requested facts in a compact table, source + dataprint age line, and flags (outdated version, unregistered extensions, backup disabled, unexpected admins). Offer a plain-text `add_ticket_note`.

## How this feeds tickets

- **Triage**: attach the version/trunk/extension picture to a telephony ticket so the tech starts with the config.
- **Currency review**: 3CX version currency is a recurring security finding (unpatched builds have been actively exploited) — surface with the as-of date, recommend the tech confirm live.
- **Incident correlation**: a call-quality or outage issue after a detected trunk/version change is a lead — label it time-adjacency, not proven cause.

## Guardrails

- Read-only and telephony-safe: this skill never dials, routes, ports, or controls calls, and never edits 3CX config. Voice/telephony control is out of scope for agent tools. Remediation is a tech's job in the console.
- Never paste SIP credentials or trunk secrets into notes even if the dataprint exposes them — reference "credentials on file in <doc platform>".
- Always state dataprint age; PBX config changes — an old dataprint on an active telephony incident deserves a "verify live" caveat.
- If no 3CX launchpoint exists, degrade per the access pattern: docs → ticket history → "verify in console".
- Plain-text notes only.
