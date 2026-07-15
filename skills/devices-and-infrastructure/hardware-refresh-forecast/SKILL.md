---
name: Hardware Refresh Forecast
description: Build a 4–5-year refresh-cycle workbook for a client — which devices cross the age threshold in each upcoming period and what the per-client refresh budget looks like. Use for "refresh forecast for <client>", budget planning, or vCIO roadmap prep.
category: Devices & Infrastructure
tools: [search_ninjaone_devices, get_ninjaone_device, search_itglue, add_ticket_note]
---

# Hardware Refresh Forecast

Turns fleet age data into a forward budget: devices grouped by the period they hit refresh age on a 4–5-year cycle, with unit counts and an estimated spend the client can plan against.

## When to use

- "Which of <client>'s machines are due for replacement, and when?"
- Annual budget or vCIO roadmap prep needs a refresh line item.
- After a Warranty/EOL report flags an aging fleet and the client asks "what will this cost us over the next few years?"

## Steps

1. Confirm the assumptions before computing, and ask only if unstated: refresh cycle length (default 4 years for laptops, 5 for desktops, 5 for servers), forecast horizon (default 3 years, by year or quarter), and per-unit budget figures. If the requester gives no unit costs, produce the forecast in unit counts and mark the spend column "pending unit pricing" — do not invent prices.
2. Pull the fleet via `search_ninjaone_devices` and per-device details with `get_ninjaone_device`. Verify class per device (laptop/desktop/server drive different cycles) — do not trust node_class filters. Flag result caps; a truncated fleet makes the budget wrong, so page to full coverage or state the floor.
3. Establish each device's age from the strongest signal: IT Glue purchase/deploy date (`search_itglue`) first, CPU-generation release window second, first-seen/OS-install date as a floor. Record which signal was used per device.
4. Exclude noise: devices offline 30+ days (verify still in service before budgeting their replacement) and devices already flagged for decommission in tickets or docs.
5. Compute the workbook: for each forecast period, the devices crossing the cycle threshold in that period (device, class, age signal, current age), unit count per period, and spend per period where unit costs exist. Add a "past due" bucket for devices already beyond cycle — these front-load the first period.
6. Fold in known accelerators: devices terminally blocked from the current OS path (from a Win11 Readiness run, if available) move to the earliest period regardless of age.
7. Output the forecast as a period-by-period table plus a one-paragraph summary (fleet size, average age, past-due count, first-period spend). Offer a plain-text summary via `add_ticket_note`, and note the client-facing version should be sanitized through the Client-Facing Device Report skill.

## Guardrails

- Every age is labeled with its evidence signal; floors are stated as floors. A budget built on invented ages is worse than none.
- Never invent unit pricing, vendor quotes, or procurement timelines — counts are facts, dollars require inputs.
- Possibly retired devices are excluded and listed for verification, not silently budgeted.
- This is a planning artifact, not a quote — say so in the output.
- Result-cap honesty on all fleet listings.
