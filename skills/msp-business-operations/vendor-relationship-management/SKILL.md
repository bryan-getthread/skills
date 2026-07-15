---
name: Vendor Relationship Management
description: Keep the MSP's own vendor relationships in order — distributor and vendor inventory, RMA processes per vendor, license and support-contract renewal tracking, and current escalation contacts. Use for "who do we call about X", "when does our <vendor> agreement renew", or an RMA that needs filing.
category: MSP Business Operations
tools: [search_tickets, search_knowledge_base, search_itglue, search_hudu, create_ticket, update_ticket, add_ticket_note, send_approval, schedule_ticket, web_search]
---

# Vendor Relationship Management

Treats the MSP's suppliers — distributors, software vendors, hardware manufacturers, its own tool stack — as managed relationships instead of tribal knowledge: a maintained inventory of what's bought from whom, each vendor's RMA path with the details that make returns succeed, renewal dates tracked as tickets before they auto-renew or lapse, and escalation contacts that are current when an incident needs them.

## When to use

- "How do we RMA this switch — what's <vendor>'s process and do we have the entitlement?"
- "What renews this quarter?" / "When does our RMM contract renew and what are we paying?"
- "Who's our account rep at <vendor>?" / "What's the severity-1 support number for <vendor>?"
- Building the vendor inventory in the first place, or auditing it after it's drifted.

## Steps

1. Establish the source of truth: vendor records live in the documentation platform (search_itglue / search_hudu / search_knowledge_base). Each vendor record should carry: what we buy, account/customer numbers, agreement term and renewal date, auto-renew and notice-period terms, support entitlement level, the RMA path, escalation contacts (rep, support tiers, severity-1 line), and the portal URL. If records are missing or stale, building them is the first deliverable.
2. **RMA handling** (when a return is the ask):
   - Pull the vendor's RMA process from documentation, then verify against the vendor's current support pages (web_search) — RMA portals and requirements change without notice.
   - Confirm entitlement first: serial number, purchase date/invoice (often via the distributor, not the manufacturer), and warranty/support status. An RMA filed without proof-of-purchase is a week of round-trips.
   - Track the RMA as a ticket: case number, ship-by dates, advance-replacement vs return-first terms, and where the replacement must go. Note whether the failed unit holds data that needs wiping before shipment — returns are a quiet data-leak channel.
3. **Renewal tracking:**
   - Inventory every agreement with a date: software licenses, support contracts, distributor terms, the MSP's own tool stack (PSA, RMM, docs, security tooling).
   - Create a renewal ticket per agreement (create_ticket, schedule_ticket) dated ahead of the notice deadline — not the renewal date; the notice window is where negotiating leverage and cancellation rights live. 60–90 days ahead for anything negotiable.
   - Each renewal ticket carries the decision inputs: current cost, seat/usage reality (are we paying for shelfware?), satisfaction notes from the period's support tickets, and alternatives worth a look. Route the renew/renegotiate/replace decision through the owner (send_approval for the spend).
4. **Escalation contacts:** keep the who-to-call current — account rep, support tiers with actual response commitments, and the severity-1 path. After any incident where a contact turned out stale, fix the record the same day; that is the maintenance loop.
5. **Relationship memory:** log significant vendor interactions (missed SLAs, great saves, pricing promises made verbally) as notes on the vendor's tracking ticket — the desk's memory at renewal time is worth real money.
6. Periodic review (quarterly fits most): sweep for agreements without renewal tickets, contacts unverified in 12+ months, and support contracts nobody has used (evidence for downgrade) or leaned on hard (evidence the tier is right).

## Guardrails

- Vendor account numbers, portal credentials, and license keys live in the vault — never in ticket notes. The vendor record points at the vault entry.
- Verify vendor processes (RMA paths, support numbers, portal URLs) against the vendor's current documentation at time of use; stale process notes are flagged and corrected, not followed blind.
- Spend decisions are the owner's: renewals, upgrades, and replacements route through approval. You assemble the decision inputs; you never commit money or accept terms.
- Auto-renew clauses with notice periods are the trap this skill exists for — always record the notice deadline as the action date, and say when a notice window has already closed.
- Keep vendor performance notes factual and dated ("missed committed 4-hour response on <date>, case <n>") — they may end up in a negotiation.
- Do not conflate this with client-owned vendor relationships (a client's ISP or LOB vendor) — those belong on the client's tickets and documentation, not the MSP's vendor inventory.
