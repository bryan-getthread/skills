---
name: Vendor Spend Review
description: Leadership wants to see the MSP's own vendor-stack spend mapped per client — license and tool COGS against agreement revenue — to find margin leaks, unused spend, and clients whose stack costs more than they pay.
category: Finance & Billing
tools: [search_clients, search_tickets, search_ninjaone_devices, connectwise_rmm_search_devices]
---

# Vendor Spend Review

Map what the MSP pays vendors (RMM, AV/EDR, backup, email security, productivity licensing, and the rest of the stack) to the clients that consume it, and set that cost against each client's agreement revenue — surfacing margin leaks: clients whose stack cost eats their margin, tools paid for but not deployed, and spend attached to offboarded clients. License Cost Optimization hunts unused licenses within a vendor; this is the portfolio view across the whole stack and the revenue line.

## When to use

- "What does our stack actually cost us per client?" / "where's our margin going?"
- An annual or quarterly vendor-spend review before renewals.
- "Are we still paying for tools on clients we lost?"

## Steps

1. Confirm scope (whole client base or named clients) and gather the inputs Thread does not hold: the vendor list with unit pricing and billed quantities (from vendor invoices/portals — ask the requester to paste or upload), and per-client agreement revenue. **Vendor pricing and revenue come from the requester; nothing here is guessed.** With neither, this review cannot run — say so.
2. Establish per-client consumption for the countable vendors: endpoint-based tools via RMM device counts (search_ninjaone_devices / connectwise_rmm_search_devices, with the billable-device filter stated), and user-based tools via seat counts the requester provides or onboard/offboard ticket movement (search_tickets) as a proxy — labeled proxy where used.
3. **Allocation** — per client: sum of (units consumed × unit cost) per vendor = stack COGS. Where a vendor bill cannot be split by client (flat platform fees), allocate by stated rule (per endpoint or per client, requester's choice) and label allocated rows as allocations, not measurements.
4. **Margin read** — per client: revenue, stack COGS, COGS share of revenue, sorted worst-first. Flag: clients whose stack cost exceeds ~30% of agreement revenue (threshold adjustable — state it), tools billed for more units than any client consumes (shelfware), and spend mapped to inactive/offboarded clients (pure leak).
5. **Leak list** — each leak with its evidence: the count mismatch, the source, and the monthly cost of the leak. Aggregate small leaks into a total rather than enumerating every $4 line.
6. Output: per-client margin table, the leak list with monthly totals, top 3 actions (renegotiate/right-size a vendor, reprice a specific client tier, cancel orphaned spend), and a methodology note — input sources and as-of dates, allocation rules, proxies used, searches run, caps hit.

## Guardrails

- Labor cost is deliberately out of scope — this is stack COGS only; say so in the output so nobody reads the margin column as full profitability (Agreement Profitability covers the labor side).
- Every cost figure traces to a requester-supplied price and a stated count source; allocations and proxies are labeled as such, never blended silently with measured numbers.
- Leadership-facing: client margin data does not go into client-visible artifacts or ticket notes.
- Over-consumption (client using more units than the MSP buys) is flagged as compliance/true-up risk with the same seriousness as leaks — cross-reference Seat Count True-Up to bill it properly.
- Never present capped device/ticket searches as complete counts; disclose caps and resolve before computing money.
- This skill reports; it cancels nothing and renegotiates nothing. Never convert a recommendation into a completed action.
- Degradation: with no RMM integration, endpoint counts come from the requester or ticket movement only — labeled weaker basis.
