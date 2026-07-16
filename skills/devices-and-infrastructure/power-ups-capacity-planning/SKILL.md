---
name: UPS / Power Capacity Planning
description: Size UPS units and forecast battery/unit replacement per site — connected load vs. capacity, runtime, and battery age — so power protection is adequate and replacements are budgeted before failure. Use when someone asks to right-size a UPS, plan battery replacements, or review power protection at a site.
category: Devices & Infrastructure
tools: [liongard_launchpoint, liongard_metric, liongard_timeline, search_ninjaone_devices, search_itglue, web_search, create_ticket, add_ticket_note]
---

# UPS / Power Capacity Planning

Make sure each site's UPS actually protects what's plugged into it and that aging batteries/units are replaced on a plan, not after an outage. Compare connected load to capacity, estimate real runtime, and forecast replacements.

## When to use

- "Is the UPS at <site> big enough for what's on it?"
- Planning battery or UPS-unit replacements across sites.
- After adding equipment to a rack, to confirm the UPS still covers it.
- A power-protection posture review before budget season.

## Steps

1. Inventory the UPS units per site: model, VA/watt rating, install date, and battery age. Pull documented power inventory (`search_itglue`) and, where Liongard runs, `liongard_launchpoint` filtered by the UPS/power systemType (e.g. "APC"/network management card inspectors) with `liongard_metric` for load %, battery status, and self-test results; `liongard_timeline` for recent battery/self-test events. Confirm the inspector last ran and state dataprint age.
2. Establish connected load vs. capacity: what's on each UPS (from documentation and the rack layout) and the current load percentage. Flag units running above ~70-80% load (little headroom, shortened battery life) and units badly oversized or undersized for their load.
3. Estimate runtime for the actual load and compare it to the requirement — is it enough for graceful shutdown, or for ride-through until generator/failover? A UPS that only buys 90 seconds for a load that needs a 5-minute shutdown is undersized in practice even if the VA rating "fits".
4. Forecast replacements: batteries typically need replacement on a multi-year cadence (verify the specific model's rated battery life and replacement interval against the vendor's current specs with `web_search` — don't state a battery lifespan from memory). Rank each unit: Replace battery now (aged out or failing self-test), Replace battery this cycle, Replace unit (EOL or chronically over-loaded), or OK.
5. Cross-reference the vendor-runbooks/apc-ups-alerts skill for handling live UPS alerts, and the storage/hardware-refresh skills where the UPS refresh rides along with a broader hardware plan.
6. Output a per-site plan: unit, rating, load %, estimated runtime vs. requirement, battery age, self-test state, ranking, and recommended action with a window. Post to the ticket as a plain-text note (`add_ticket_note`) or `create_ticket` for the replacement plan.

## Guardrails

- Verify UPS model ratings and battery-life/replacement intervals against current vendor specs — never state runtime or battery lifespan from memory.
- "VA rating fits" is not "runtime is adequate" — always compare estimated runtime for the real load against the shutdown/ride-through requirement.
- Trust Liongard/RMM data only after confirming the inspector last ran; report dataprint age. Where a UPS has no network card/inspector and no documented load, mark it "unverified — inventory only" rather than estimating load.
- This skill plans and forecasts; it does not change UPS config or dispatch a replacement.
- Notes posted to tickets are plain text — no markdown, no emojis.
