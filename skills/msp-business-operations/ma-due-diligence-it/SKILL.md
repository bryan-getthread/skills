---
name: M&A Due Diligence IT
description: IT due diligence when an MSP acquires or merges with another book of business — client-stack census, contract and agreement inventory, tool-consolidation map, and the day-one risk list. Use for "we're acquiring <target>, what do we need to look at?" or integration planning after a signed deal.
category: MSP Business Operations
tools: [search_tickets, search_clients, search_knowledge_base, search_itglue, search_hudu, create_ticket, add_ticket_note, web_search]
---

# M&A Due Diligence IT

Structures the technical side of buying a book of business: what the target's clients actually run, what the agreements actually promise, which tools overlap with yours and what consolidation will cost, and the ranked list of things that can hurt you on day one. The output is a working due-diligence pack for the deal team — evidence where evidence exists, and explicit "unverified — target-provided" labels everywhere else.

## When to use

- "We're in diligence on acquiring <target MSP> — build the IT checklist / work through their data."
- "They sent us their client list and agreement summaries — census it and find the risks."
- Post-close: "map the tool consolidation and the day-one risk list for integration."
- Sell-side prep: the same census run on your own house before a buyer does.

## Steps

1. Establish deal stage and data reality: pre-LOI (checklist and question lists only), in-diligence (working target-provided exports), or post-close (target data may now be in your systems). Label every artifact accordingly — **nothing target-provided is treated as verified until your team has checked it**. All work happens under the deal's confidentiality posture (see guardrails).
2. **Client-stack census** — per target client, from whatever exists (their PSA/docs exports; post-close, search_clients / search_tickets / search_itglue / search_hudu once imported):
   - Seats/endpoints, core stack (identity, email, server/cloud posture, firewall/network vendor, backup, security tooling), and anything exotic (LOB apps, compliance regimes like HIPAA/CMMC, OT/vertical gear).
   - Health signals if ticket data is available: volume per client, recurring-issue families, aged backlogs — the census should distinguish well-run clients from inherited fires.
   - Flag concentration risk: revenue share of the top clients, and any client that is itself the reason for the deal.
3. **Contract & agreement inventory** — per client: agreement type (managed/AYCE, block hours, T&M), term and expiry, **assignability/change-of-control clauses** (the deal-killer detail — flag every agreement that requires client consent to transfer), pricing vs the effort the ticket data implies, and out-of-scope patterns. Also the target's own paper: vendor/tool contracts, leases, their staff's client-credential exposure.
4. **Tool-consolidation map** — target stack vs yours (PSA, RMM, docs, security, backup, remote access):
   - Per tool: keep-ours / keep-theirs / run-both-temporarily, the migration weight (RMM agent swaps and docs migration are the long poles), contract end-dates that force or fund the timing, and double-pay windows.
   - Name the data-migration risks explicitly: ticket history, docs/credentials, and client-consent needs for tooling changes in regulated environments.
5. **Day-one risk list** — ranked, each with an owner slot and a mitigation:
   - **Credential exposure:** who at the target holds client credentials; rotation plan for departing staff (run internal-it-offboarding discipline at acquisition scale).
   - Expiring/consent-requiring agreements landing near close; single-tech client dependencies (the one person who knows <client> — retention risk = client risk); EOL/unsupported infrastructure carrying implicit remediation cost; security posture gaps (no MFA, no backup verification) that become YOUR liability at close; clients already churning in the target's ticket signals.
6. Assemble the pack: census table, agreement inventory with the assignability flags up top, consolidation map with cost/timing, ranked day-one risks, and a question list for the target where data was missing. Track workstreams as tickets on a restricted board if the tenant wants execution tracking (create_ticket).

## Guardrails

- **Confidentiality is structural.** M&A leaks damage both companies: work only in the deal's restricted space, never on general boards, never in team-visible notes; refer to the target by the deal codename if one exists. If asked to analyze deal material somewhere the team can see, stop and say why.
- Label evidence honestly: target-provided vs independently verified, and dated. A diligence pack that presents seller claims as findings is worse than none.
- No valuation, price, or go/no-go opinions — you inventory risk and cost drivers; bankers, lawyers, and the owner price the deal. Assignability clauses in particular get flagged, not interpreted: contract reading is counsel's job.
- People data in diligence (target staff, salaries, retention candidates) is need-to-know deal-team material — keep it out of this pack beyond role-level dependency risk ("client X depends on one engineer"), and follow the people-canon framing when it appears.
- Do not contact the target's clients, staff, or vendors, and do not touch the target's systems — diligence works on provided data until the deal closes and integration formally begins.
- Post-close integration actions (credential rotation, tool migration, client notices) are executed by named humans under the integration plan — this skill plans and tracks; the zero-assumption rule applies to every migration step.
