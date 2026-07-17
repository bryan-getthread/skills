---
name: Automation ROI Report
description: Someone asks whether the automations already live are earning their keep — tickets touched by each flow/intent, estimated time saved, noise created — ending in a keep / kill / tune verdict per automation.
category: Reporting & Analytics
tools: [search_tickets, list_flows, list_intents, get_flow, get_intent, list_boards]
connectors: []
---

# Automation ROI Report

**When to use:** "Are our automations actually saving us time?", a quarterly automation review before building more, or when techs complain a flow is annoying and someone wants evidence either way.

## Prompt

```
Audit the automations that already exist: for each active flow and intent, how many
tickets it touched in the period, a labeled estimate of time saved, and the noise it
created — then a keep / kill / tune verdict. This skill never disables, edits, or
creates flows/intents.

1. Confirm the period (default: last full quarter). Inventory active automations via
   list_flows and list_intents; for each, read what it does (get_flow / get_intent) —
   trigger, actions, and whether it routes, gathers, or resolves. (Intent/flow tools
   are admin-only; if unavailable on this token, report that the inventory cannot be
   read and stop.)

2. TOUCH COUNTS — per automation, search_tickets for tickets showing its footprint in
   the period (matching category/board/trigger pattern, automation-applied tags or
   statuses, its notes). State which footprint signal was used; where attribution is
   fuzzy, give a RANGE, not a point count. If you cannot tell whether the automation or
   a human did the work, say so and bound the count rather than crediting the
   automation. Record caps; never present capped searches as exact touch counts.

3. TIME-SAVED ESTIMATE (labeled) — per automation: touches x minutes saved per touch,
   where the per-touch figure comes from what the automation replaces (triage-and-route
   ~2-4 min; info-gathering ~5-10 min; full resolution ~ the category's median handle
   time from a sample). Every such number carries the word "estimate" and shows its
   per-touch assumption — NEVER present modeled savings as measured hours.

4. NOISE SIDE OF THE LEDGER — per automation: tickets it routed wrong (bounced after
   auto-routing), duplicate/unnecessary notes or messages, reopened auto-resolutions,
   tech-visible clutter. Count reopened auto-resolutions against the automation
   honestly — a fast wrong answer is negative ROI.

5. VERDICTS — keep (clear positive ledger), tune (works but misroutes/over-fires — say
   what to change), kill (near-zero touches, or noise exceeds value). Zero-touch
   automations from a past era are kill candidates, not trophies. Verdicts are
   recommendations only; changes go through an admin after human review.

6. Output: one table (automation / touches / est. time saved / noise observed / verdict
   / one-line rationale), the top tune recommendations in enough detail to act on,
   total estimated hours saved clearly marked as an estimate with assumptions listed,
   and a methodology note (period, attribution signals per automation, per-touch
   assumptions, searches, caps).
```
