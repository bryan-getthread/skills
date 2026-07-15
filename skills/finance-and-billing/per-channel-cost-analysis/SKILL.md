---
name: Per-Channel Cost Analysis
description: Someone asks what it costs to serve a ticket by channel — email vs chat vs voice vs portal — using handle-time proxies, and what deflection to cheaper channels or self-service would be worth.
category: Finance & Billing
tools: [search_tickets, list_boards, search_clients]
---

# Per-Channel Cost Analysis

Estimate cost-to-serve per intake channel — email, chat/Messenger, voice, portal, and alert-generated — from ticket volume and handle-time proxies, then frame what channel-shifting and deflection would actually be worth in hours and dollars. Every cost here is a modeled estimate and is labeled as one; the deliverable is honest relative economics between channels, not fake-precise unit costs.

## When to use

- "What does a phone ticket cost us versus a chat ticket?"
- Deciding whether to push clients toward the Messenger/portal, or staff the phones differently.
- Sizing the business case for self-service (pairs with Self-Service Deflection Analysis, which measures deflection already happening; this prices what more would be worth).

## Steps

1. Confirm the period (default: last full quarter), boards in scope (list_boards), and how this desk records channel/source on tickets — a source field, channel-specific boards, or subject conventions. State the channel-attribution signal in the output; if a large share of tickets has no channel signal, report that share as "unattributed" rather than distributing it.
2. Run split searches per channel per board: volume, and the handle-time proxy — logged time per ticket where time entries exist; otherwise touches-per-ticket and open-duration as weaker proxies, labeled. Record caps.
3. Compute per channel: volume, median handle time (proxy and its source stated), and modeled cost per ticket = handle time × loaded hourly cost. The loaded rate comes from the requester; if not provided, express costs in hours only — hours are real, invented rates are not.
4. **Mix effects check** — channels carry different work: voice gets urgent/complex issues, portal gets routine requests. Compare like categories across channels where volume permits (e.g., password resets by channel) so the report does not "discover" that outages cost more than form requests. Say plainly when a channel's higher cost is mix, not channel inefficiency.
5. **Deflection value framing** — for the top shiftable categories (routine, low-emotion, high-volume), model the saving of moving N% to a cheaper channel or self-service: hours/month, labeled estimate with assumptions. Frame deflection as value honestly — deflection saves money only when the cheaper channel actually resolves the issue; a portal ticket that ends in a phone call cost more, not less. Include observed bounce-backs where visible.
6. Output: per-channel table (volume / handle-time proxy / modeled cost or hours / dominant categories), the mix caveat, the deflection model with assumptions, 2–3 recommendations with trade-offs (client experience, not just cost — some clients pay premium agreements precisely to call a human), and a methodology note — attribution signal, unattributed share, proxies, rate source, searches run, caps hit.

## Guardrails

- Everything monetary is a labeled estimate with visible assumptions; without a supplied loaded rate, report hours only.
- The mix-vs-channel distinction is mandatory — skipping step 4 produces the classic wrong conclusion ("kill the phones").
- Deflection framing must include the failure mode (bounce-back to expensive channels) and the relationship cost of forcing channels on clients who did not ask.
- Never present capped searches as exact volumes; disclose caps, and never let the unattributed share vanish silently.
- Aggregate, not enumerate — categories and counts, not ticket lists.
- Leadership-facing analysis; per-channel costs do not belong in client-visible artifacts.
