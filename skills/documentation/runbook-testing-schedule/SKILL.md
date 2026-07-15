---
name: Runbook Testing Schedule
description: Build the test cadence that keeps runbooks from rotting — criticality-based schedule, a fixed test-result capture format, and a fix-or-retire verdict for every test, so no runbook silently decays until the 2 a.m. night it's needed.
category: Documentation
tools: [search_knowledge_base, search_itglue, search_hudu, search_tickets, create_ticket, add_ticket_note]
---

# Runbook Testing Schedule

Runbooks rot: systems change, vendors redesign portals, the person who wrote step 4 leaves. This skill inventories the desk's runbooks, assigns each a test cadence by criticality, and installs the discipline that every test ends in a verdict — pass, fix, or retire — captured where the next tester can see it.

## When to use

- "Set up a testing schedule for our runbooks."
- A runbook just failed during a real incident and the desk wants to stop that recurring.
- Quarterly hygiene: "which runbooks are due for a test, and what did the last tests find?"

## Steps

1. **Inventory the runbooks:** search_knowledge_base and search_itglue / search_hudu (where enabled) for procedure-type documents — restore procedures, failover steps, escalation playbooks, maintenance sequences. In-app, the runbooks family (search_runbooks) sees Thread-native runbooks directly; over external MCP it is unavailable, so inventory via the doc searches and the requester. Scope per client or per system when the estate is large.
2. **Assign criticality — the cadence follows from it:**
   - **Critical** (invoked during incidents where failure is unaffordable: restore-from-backup, DR failover, ransomware response, major-outage escalation) → test **quarterly**, and additionally after any change to the underlying system.
   - **Important** (routine-but-consequential: onboarding/offboarding sequences, server maintenance, certificate renewal) → test **twice a year**.
   - **Routine** (convenience procedures with a safe manual fallback) → test **annually**, or opportunistically when actually used — real use counts as a test *if the result is captured*.
   Evidence for placement: search_tickets for how often incidents touch the runbook's system, and how bad those tickets get. When in doubt between two levels, pick the stricter.
3. **Define the test-result capture** — one fixed format per test, filed as a note on the test ticket and a dated annotation on the runbook itself (plain text per the Note Format Standard skill, documentation/note-format-standard):
   - SUMMARY: runbook name, test date, tester.
   - RESULT: PASS / FAIL, and per-step notes for anything that deviated ("step 3 button renamed", "step 6 credential moved").
   - TIME: how long the procedure took — drift here is early rot warning.
   - VERDICT: see step 4.
4. **Fix-or-retire verdict — every test ends in exactly one:**
   - **Pass** → stamp the runbook "last tested <date>" and schedule the next test.
   - **Fix** → the tester (or a named owner) updates the runbook *now*, while the deviations are fresh; the fix is a ticket, not a hope. Re-test only if the fixes were structural.
   - **Retire** → system gone or procedure superseded: mark the runbook retired/archived with a pointer to its replacement. A wrong runbook is worse than no runbook — it burns 20 minutes at 2 a.m. before anyone doubts it.
   "Failed but we'll get to it" is not a verdict; an untested-and-known-broken runbook is retired until fixed.
5. **Emit the schedule:** a table of runbook / criticality / cadence / last-tested / next-due / owner. If asked, create the recurring test tickets via create_ticket (one per runbook per due date, criticality-appropriate board/priority) so the schedule lives on a board rather than in a document nobody reopens.

## Guardrails

- This skill schedules and captures — it never executes a runbook. Tests that touch production (restores, failovers) are human-run, in approved windows, per the desk's change process.
- Criticality assignments and test ownership are recommendations until the requester confirms them; do not create ticket load unasked.
- Never mark a runbook tested without a captured result — an unrecorded test didn't happen.
- Retirement is flagged-and-confirmed, not deleted: this skill never deletes documentation; a human archives with the replacement pointer.
- Result-cap honesty: the inventory may be partial where searches cap — say which scope was actually covered.
- Degradation: without IT Glue/Hudu, inventory from the Thread KB + requester list only; the runbooks tool family is in-app SuperAgent only — over external MCP say the Thread-native runbook registry wasn't directly enumerable.
