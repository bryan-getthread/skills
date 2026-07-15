---
name: Seat Count True-Up
description: Monthly true-up time — compare each per-seat/per-device agreement's actual counts (RMM devices, onboard/offboard tickets) against what billing says, and produce the evidence pack for adjustments.
category: Finance & Billing
tools: [search_tickets, search_clients, search_ninjaone_devices, connectwise_rmm_search_devices]
---

# Seat Count True-Up

For each client billed per seat or per device, establish what the count *actually is* this month — from RMM device inventory and onboarding/offboarding tickets — and compare it to the count being billed, producing a true-up evidence pack: who is under-billed, who is over-billed, and the tickets/devices that prove it. License Billing Reconciliation is the per-license (M365, AV, backup SKU) version of this; run that when the dispute is about licenses rather than seats/endpoints.

## When to use

- The monthly billing run is coming and someone asks to "true up the seat counts."
- "Are we billing <client> for the right number of users/devices?"
- Finance found revenue flat while the desk swears clients grew.

## Steps

1. Confirm scope — all per-seat/per-device clients or one client (search_clients) — the billing month, and the source of the *billed* counts. Billed quantities live in the billing system, not in Thread: ask the requester to paste or upload the current billed count per agreement. **No billed counts = no true-up**; offer the actual-count half of the pack alone, clearly labeled as such.
2. Per client, establish the actual count from the strongest available sources: RMM device inventory (search_ninjaone_devices or connectwise_rmm_search_devices — whichever integration this tenant has), filtered to billable device classes per the agreement (exclude spares/decommissioned; state the filter used).
3. Corroborate with movement tickets: search_tickets for onboarding/new-user and offboarding/termination tickets for the month. Movement should explain the delta between last month's and this month's counts; unexplained deltas get flagged rather than smoothed over.
4. Per agreement, compare actual vs billed: delta, direction (under-billed = revenue leak; over-billed = client-trust/credibility risk — flag both with equal seriousness), and the monetary impact if the per-unit rate is provided (labeled with the rate used).
5. Build the **evidence pack** per discrepancy: the device list or count with source and as-of date, the onboard/offboard ticket references, and a one-line narrative ("3 users onboarded in tickets <refs>, never added to billing"). Evidence must be strong enough to survive the client asking "prove it."
6. Output: a true-up table (client / billed / actual / delta / direction / monthly impact), the per-discrepancy evidence packs, a recommended adjustments list for the billing admin, and a methodology note — sources per client, device filters, as-of dates, searches run, caps hit.

## Guardrails

- This skill produces the evidence pack; it never changes billing, quantities, or invoices. Never convert a recommended adjustment into a completed one.
- Over-billing is reported as prominently as under-billing — a true-up that only finds revenue is not credible.
- Device counts are as-of-date snapshots from a stated source with stated filters; a stale RMM agent is not a billable seat — flag devices unseen for 30+ days separately instead of counting them either way silently.
- Never present capped device or ticket searches as complete counts; disclose caps and re-split until counts are exact, since billing math cannot run on floors.
- Degradation: if no RMM integration is enabled for this tenant, say so and fall back to movement tickets only, labeled as a weaker count basis.
- Monetary impacts use only rates the requester supplied — never assumed per-seat pricing.
