---
name: Technician KPI Pack
description: A technician or their lead wants a personal scorecard for a period — first-contact resolution, touch efficiency, reopen rate, and CSAT — as a reusable template, not a one-off judgment.
category: Reporting & Analytics
tools: [search_tickets, search_members]
---

# Technician KPI Pack

Produce one technician's personal scorecard for a period: FCR, touch efficiency, reopen rate, and CSAT, each with the definition used printed next to the number. This is the quantitative companion to My Performance Review (personal-productivity) — that skill builds the narrative self-review; this one builds the numbers page. For a manager's evaluation with coaching angles, use Tech Performance Review instead.

## When to use

- "Pull my KPI pack for last quarter" / "build my scorecard."
- A tech prepping numbers for a self-review or 1:1 (pair with My Performance Review).
- A lead standardizing what "the tech scorecard" contains so everyone is measured the same way.

## Steps

1. Confirm the technician (search_members) and the period (default: last full quarter). If the requester is the tech themselves, say so in the output header — self-pulled scorecards read differently than manager-pulled ones.
2. Run split searches per signal: tickets resolved by the tech, tickets assigned, reopened tickets, and tickets with survey/sentiment responses. Record any result caps.
3. Compute the four core metrics, each with its definition inline:
   - **FCR** — tickets the tech resolved directly from first assignment with no reassignment or escalation, over tickets first-assigned to them. State this definition in the output (see First Contact Resolution Report for the desk-wide version).
   - **Touch efficiency** — average touches (notes/updates) per resolved ticket; lower is usually better but complex work legitimately runs high, so pair it with the tech's ticket-type mix.
   - **Reopen rate** — resolved tickets reopened within the period, over resolutions. Exclude reopens caused by client-side delays where identifiable.
   - **CSAT** — survey score with the response count shown next to it; if fewer than ~10 responses, label the score low-confidence rather than presenting it as a stable number.
4. Add prior-period comparison where data exists; never fabricate a baseline.
5. Output the scorecard as a compact table (metric / definition / value / prior / direction), two sentences of context on the tech's ticket mix (role, escalation tier, project vs reactive), and a methodology note: period, searches run, caps hit, and proxies used.

## Guardrails

- Role-aware benchmarking is mandatory: an escalation engineer's FCR and touch counts are not comparable to a triage tech's. Never compare this scorecard against another tech's without matching roles, and never rank on volume alone.
- Print every metric's definition next to its value — a scorecard number without its definition is how metric disputes start.
- Low survey N is stated plainly, not hidden ("2 responses — treat as anecdote, not a score").
- Never present capped searches as exact totals; disclose caps.
- This is a template for a recurring artifact: keep sections and definitions identical run-to-run so periods stay comparable, and note in the output if a definition changed since the last run.
