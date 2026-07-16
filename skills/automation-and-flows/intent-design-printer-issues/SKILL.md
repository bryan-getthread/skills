---
name: Printer Issues Intent Design
description: Build the printer-problems intent — the top three self-help fixes first, then a ticket that already carries the diagnostics. Use when asked to "build a printer intent", "deflect printer tickets", or when printing ranks high in Intent Mining.
category: Automation & Flows
tools: [list_intents, get_intent, create_intent, update_intent, set_variation_arguments, set_variation_replies, update_variation, search_tickets, search_knowledge_base]
---

# Printer Issues Intent Design

Guide the admin through building a printer intent that walks the user through the three fixes that resolve most printer tickets, and — when they fail — creates a ticket with the diagnostics already captured so the tech skips the twenty questions.

## When to use

- "Build an intent for printer problems."
- "Printer tickets are constant and always the same three fixes."
- Intent Mining ranked printing as a high-volume, high-automatability candidate.

## Design

**Trigger phrases (adapt to real ticket language):** "printer not working", "can't print", "cant print", "printer offline", "print job stuck", "nothing comes out of the printer", "printer says offline", "printing problem", "printer error", "documents stuck in queue", "printer won't print".
Near-miss watch: "need a new printer" is a hardware request; "add a printer for a new user" is onboarding/setup; "scan to email broken" may be a separate scan/email flow — decide with the admin whether to include it.

**Arguments to collect (before or during self-help):**
- Which printer (name/location placeholder: <device>, "the one in <location>").
- One user or everyone nearby (scopes device vs. server/queue problem).
- What happens: nothing / error message (capture exact text) / prints garbage / stuck in queue.
- What they already tried.

**Reply flow (self-help ladder — top 3 fixes):**
1. **Restart the printer** — power off, wait 30 seconds, power on; then retry.
2. **Clear the local queue** — cancel the stuck job(s) and reprint (give plain steps for the client's OS mix).
3. **Reconnect/restart from the computer side** — restart the computer, or remove/re-add or set-default steps if the client's environment supports user-level fixes.
After each rung: "did that fix it?" — stop the moment it works and close as deflected.
4. If all three fail, or multiple users are affected → create a ticket carrying: printer identity/location, scope, exact error text, rungs attempted and results. Multiple-users-affected skips straight to the ticket (likely a queue/server issue).

**Handoff rule:** no credential steps, no driver installs requiring admin rights in self-help — those go to the ticket. Anything smelling of print-server outage (everyone affected) short-circuits self-help.

**Variation hooks (per client):** printer fleet names/locations, whether users can add printers themselves, managed-print vendor to mention, OS mix for step phrasing.

**Success metric:** deflection rate on printer conversations (resolved without a ticket) plus diagnostics completeness on the tickets still created. A realistic early target is meaningful single-user deflection; queue/server issues will and should still ticket.

## Steps

1. `list_intents` — check for an existing printer intent; prefer updating.
2. `search_tickets` for recent printer tickets; confirm the top three fixes for THIS desk from resolution notes — replace any rung that the data says rarely works.
3. `search_knowledge_base` for existing printer self-help articles; link rather than restate where they exist.
4. Draft the full spec: triggers, arguments, the 3-rung ladder with stop conditions, the diagnostics block for the ticket, variations. Test plan: 5 should-match, 3–5 should-not (hardware-request and new-user-setup near-misses).
5. Show the draft; on explicit confirmation call `create_intent` then the variation tools.
6. Report what was created, restate the test plan, recommend activation after tests pass. Do not activate.

## Guardrails

- Intent tools are admin-only; if absent, output the complete written spec for an admin to apply.
- Self-help steps must be safe for a non-technical user: no admin credentials, no registry/driver surgery, no steps that could take other users' printing down.
- Ground the three fixes in this desk's resolution data where possible; do not present generic fixes as "what usually works here" without evidence.
- Multiple-users-affected always escalates — never keep a probable outage in a self-help loop.
- Confirm before any write; never activate on the member's behalf. Diagnostics block in plain text.
