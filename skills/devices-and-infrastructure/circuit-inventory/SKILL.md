---
name: Circuit Inventory
description: Build or refresh the inventory of a client's circuits — internet, WAN/MPLS/SD-WAN underlays, and POTS-replacement lines — with carrier, circuit ID, account reference, site, bandwidth, and contract/renewal dates. Use for "what circuits does <client> have", renewal planning, or when an outage reveals nobody knows the circuit ID.
category: Devices & Infrastructure
tools: [search_itglue, search_hudu, search_tickets, add_ticket_note, create_ticket, schedule_ticket]
---

# Circuit Inventory

The living register of every line a client leases: which carrier, which circuit ID, which site it lands at, what it costs the client to be without it, and when the contract renews — assembled from documentation and ticket history, with the gaps named.

## When to use

- "What circuits does <client> have and who are the carriers?"
- An outage stalls because nobody can find the circuit ID or carrier account reference.
- Contract-renewal planning ("which circuits renew this year?").
- POTS-line sunset work: finding the remaining copper/analog lines (alarms, fax, elevators) and their replacements.

## Steps

1. Pull documented circuits: `search_itglue` / `search_hudu` for circuit, ISP, WAN, and telecom documentation per site. Capture per circuit: site, carrier, service type (DIA/broadband/MPLS/SD-WAN underlay/POTS-replacement), circuit ID, carrier account number *by reference* (which doc holds it — never copy account passwords or PINs into your output), bandwidth, static IP assignments by reference, contract term and renewal/expiry date, and the support phone number for outages.
2. Mine ticket history for undocumented circuits: `search_tickets` for carrier names and outage tickets per site — a carrier that appears in outage history but not documentation is a gap. Report capped searches as "at least N".
3. Special sweep for POTS and analog survivors: alarm lines, fax lines, elevator phones, fire-panel dialers. These hide outside IT documentation; flag any site where the docs are silent on analog lines as "unverified — confirm on site", since they carry life-safety implications during carrier migrations.
4. Build the per-site inventory table: carrier, type, circuit ID, bandwidth, renewal date, doc reference, last-outage date from ticket history. Mark every cell you could not source; empty beats invented.
5. Renewal calendar: list circuits by renewal date. For any renewal inside the client's typical renegotiation window (commonly 90–180 days out), recommend creating a dated follow-up (`schedule_ticket` or `create_ticket`) so renewals surface as work items, not surprises.
6. Output the inventory plus a gap list (undocumented circuits, missing IDs, unknown renewal dates, unverified analog lines) with a recommended documentation task per gap. Offer to post the summary as a plain-text note (`add_ticket_note`) and open the documentation/renewal tickets. Updating the documentation platform itself is a handoff.

## Guardrails

- Account numbers, PINs, and portal credentials are referenced by document location only — never reproduced in notes or output.
- Never invent a circuit ID, renewal date, or bandwidth figure to complete the table. Unknowns are listed as unknowns.
- Renewal dates sourced from documentation of unknown age get a "verify with carrier" caveat — auto-renew clauses make stale dates costly.
- Contract and pricing decisions are account management's call; this skill surfaces dates and facts, it does not recommend switching carriers.
- If the tenant has no documentation integration, build what you can from ticket history, label it "reconstructed, unverified", and lead with the recommendation to document properly.
- Notes are plain text — no markdown, no emojis.

## See also

- `isp-outage-tracking` — the consumer of this inventory during an outage.
- `network-device-inventory` — the equipment-side counterpart (this skill covers the lines, that one the boxes).
