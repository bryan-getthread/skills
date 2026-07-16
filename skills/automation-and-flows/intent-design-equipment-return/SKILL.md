---
name: Equipment Return Intent Design
description: Build the equipment-return intent — capture what hardware, from whom, the reason, and the shipping/pickup logistics so the return ticket arrives ready to coordinate. Use when asked to "build an equipment return intent", "automate device return requests", or when returns rank high in Intent Mining.
category: Automation & Flows
tools: [list_intents, get_intent, create_intent, update_intent, set_variation_arguments, set_variation_replies, update_variation, search_tickets]
---

# Equipment Return Intent Design

Guide the admin through building an intent whose whole job is complete logistics intake for returning company equipment — most often from a departing or remote user. The intent does not deflect; it makes the return ticket coordinate-ready: what's coming back, from where, and how.

## When to use

- "Build an intent for equipment returns."
- "Remote-return tickets are always missing the shipping address or the asset — fix the intake."
- Intent Mining flagged device returns (offboarding, remote, refresh) as a recurring request.

## Design

**Trigger phrases (adapt to real ticket language):** "return my laptop", "how do I send back my equipment", "returning company gear", "ship back my device", "employee leaving — collect their laptop", "return the old monitor", "send back the loaner", "equipment pickup".
Near-miss watch: "employee is leaving" broadly belongs to the offboarding intent — this one is scoped to the *return of hardware*; "I need new equipment" belongs to the hardware-request intent.

**Arguments to collect:**
- Which equipment (device type + asset tag / serial if the user has it).
- Whose equipment / who is returning it (self or a departing user, placeholder <user>).
- Reason: departure, remote-to-office, refresh/upgrade swap, loaner return.
- Return method and logistics: pickup vs. prepaid shipping label, the current physical location/address, and desired timeframe.
- Condition notes / accessories included (charger, dock, peripherals).

**Reply flow:**
1. Collect the arguments; confirm the equipment and the return method back to the requester.
2. Create the ticket with a structured plain-text block and route to the asset/logistics or offboarding workflow the client uses.
3. Reply with what happens next (e.g. a label or pickup will be arranged) — do not promise a specific carrier date or that a label is already issued.

**Handoff rule:** issuing shipping labels, scheduling couriers, and updating asset records are human/workflow actions — the intent only performs intake and routing.

**Variation hooks (per client):** whether returns use prepaid labels or courier pickup, the return depot address, data-wipe / chain-of-custody requirements before return, which board or flow receives the ticket.

**Success metric:** first-touch completeness — percentage of return tickets arriving with equipment, returner, method, and location captured (zero follow-up questions). Secondary: time from request to logistics arranged.

## Steps

1. `list_intents` — check for an existing return/offboarding/asset intent; prefer `update_intent` over a duplicate and avoid overlap with the offboarding intent.
2. `search_tickets` for recent return tickets; mine phrasing (triggers) and the follow-ups techs asked (usually address, asset tag, accessories → arguments).
3. Draft the full spec: triggers, arguments, confirmation reply, routing target, variations, and a test plan (5 should-match, 3–5 should-not including a hardware-request near-miss). Show before any write.
4. On explicit confirmation: `create_intent`, then `set_variation_arguments` / `set_variation_replies` / `update_variation`.
5. Report what was created, restate the test plan, recommend activation only after tests pass. Do not activate.

## Guardrails

- Intent tools are admin-only; if absent, degrade to a complete written spec for an admin to apply.
- The intent captures and routes — it does not issue labels, book couriers, or update asset records; those follow the client's logistics workflow.
- Do not invent the return address, carrier, or data-wipe policy — placeholder anything unknown and flag it before activation.
- Never promise a pickup date or that a label is already sent; state that logistics will be arranged.
- Confirm before any write; never activate on the member's behalf. Ticket field block in plain text (PSA-safe, no markdown).
