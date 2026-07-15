---
name: Automation ROI Report
description: Someone asks whether the automations already live are earning their keep — tickets touched by each flow/intent, estimated time saved, noise created — ending in a keep / kill / tune verdict per automation.
category: Reporting & Analytics
tools: [search_tickets, list_flows, list_intents, get_flow, get_intent, list_boards]
---

# Automation ROI Report

Audit the automations that already exist: for each active flow and intent, how many tickets it touched in the period, a labeled estimate of time saved, and the noise it created — then a keep / kill / tune verdict. Intent Coverage Report maps what exists against demand; this skill asks whether what exists is actually working.

## When to use

- "Are our automations actually saving us time?"
- A quarterly automation review, or before building more ("is what we have even earning out?").
- Techs complain a flow is annoying and someone wants evidence either way.

## Steps

1. Confirm the period (default: last full quarter). Inventory active automations via list_flows and list_intents; for each, read what it does (get_flow / get_intent) — trigger, actions, and whether it routes, gathers, or resolves.
2. **Touch counts** — per automation, search_tickets for tickets showing its footprint in the period (matching category/board/trigger pattern, automation-applied tags or statuses, its notes). State per automation which footprint signal was used; where attribution is fuzzy, give a range, not a point count. Record caps.
3. **Time-saved estimate (labeled)** — per automation: touches × minutes saved per touch, where the per-touch figure comes from what the automation replaces (e.g., triage-and-route ≈ 2–4 min; info-gathering ≈ 5–10 min; full resolution ≈ the category's median handle time from a sample). Every such number carries the word "estimate" and shows its per-touch assumption — never present modeled savings as measured hours.
4. **Noise side of the ledger** — per automation, look for: tickets it routed wrong (bounced after auto-routing), duplicate or unnecessary notes/messages, reopened auto-resolutions, and tech-visible clutter. An automation's cost is real even when its savings are real.
5. **Verdicts** — keep (clear positive ledger), tune (works but misroutes/over-fires — say what to change), kill (near-zero touches, or noise exceeds value). Zero-touch automations from a past era are kill candidates, not trophies.
6. Output: one table (automation / touches / est. time saved / noise observed / verdict / one-line rationale), the top tune recommendations in enough detail to act on, total estimated hours saved for the period clearly marked as an estimate with assumptions listed, and a methodology note — period, attribution signals per automation, per-touch assumptions, searches run, caps hit.

## Guardrails

- Estimates are labeled estimates, everywhere, with assumptions shown — this report's credibility dies the first time a modeled number gets quoted as a measured one.
- Attribution honesty: if you cannot tell whether the automation or a human did the work, say so and bound the count rather than crediting the automation.
- Verdicts are recommendations only — this skill never disables, edits, or creates flows/intents; never convert a recommendation into a completed action. Changes go through an admin after human review.
- Never present capped searches as exact touch counts; disclose caps.
- Count reopened auto-resolutions against the automation honestly — a fast wrong answer is negative ROI.
- Intent/flow tools are admin-only; if unavailable on this token, report that the inventory cannot be read and stop.
