---
name: Automation Owner Monthly Ritual
description: The automation engineer's monthly runbook — zero-touch opportunity mining, intent-coverage report, automation ROI, a flow-debugger sweep of failures, and one new automation shipped — run when the automation owner says "monthly automation pass" or "what should we automate next".
category: Role Rituals
tools: [search_tickets, list_flows, get_flow, list_intents]
---

# Automation Owner Monthly Ritual

The automation program's monthly heartbeat: find the next opportunity, measure coverage and ROI, fix what's silently failing, and ship exactly one improvement. The ritual ends with something in production, not a backlog.

## When to use

- "Run the monthly automation pass" / "what should we automate next" / "automation health check."
- Start of month, on the automation owner's program slot.
- After a volume spike, to see what the humans absorbed that a flow should have.

## Steps

1. **Opportunity mining (30 min)** — run `skills/reporting-and-analytics/zero-touch-opportunity-mining` over the month's tickets: the highest-volume patterns humans still handle end-to-end, ranked by frequency × handling time.
2. **Intent coverage (20 min)** — run `skills/reporting-and-analytics/intent-coverage-report`: what share of inbound matched an intent, top unmatched clusters, intents that misfired. Unmatched clusters cross-check against step 1's opportunities.
3. **ROI report (20 min)** — run `skills/reporting-and-analytics/automation-roi-report`: what existing automations saved this month, which flatlined, which regressed. A flatlined automation is a candidate for fixing or retiring — dead flows are debt.
4. **Failure sweep (30 min)** — run `skills/automation-and-flows/flow-debugger` across flows with failed or anomalous runs this month. Fix the small ones now; ticket the big ones. A flow that fails silently for a month erodes trust in the whole program.
5. **Ship one (60+ min)** — pick the single best candidate from steps 1–3 and ship it: an intent via `skills/automation-and-flows/intent-builder`, a flow via `skills/automation-and-flows/flow-builder`, or a tune via `skills/automation-and-flows/triage-agent-tuning`. Unattended pieces follow `skills/automation-and-flows/unattended-output-discipline`. Shipped means live with a success metric named — not drafted.
6. Output the monthly automation card: top opportunities (ranked), coverage movement, ROI verdict per automation family, failures fixed/ticketed, and the one thing shipped with its metric.

## Time-short version

Steps 4 and 5 — fix what's broken, ship the one thing. Mining, coverage, and ROI reporting can trail; note the deferral and reuse last month's ranked list for the ship pick.

## Guardrails

- Ship one means one, live, with a named success metric checked at next month's ritual — the ritual fails honestly ("nothing shipped: <reason>") rather than counting a draft as shipped.
- New unattended automations start conservative: high-confidence triggers, do-nothing-when-unsure posture, and a rollback note in the flow description.
- Never widen an automation's write scope as a side effect of debugging; scope changes are explicit decisions with approval.
- ROI numbers disclose their assumptions (time-saved-per-run estimates are estimates); result caps on month-scale mining are disclosed.
- Skip-with-note beats fake completion.
