---
name: Presales Technical Brief
description: When an engineer is joining a sales call for an existing client and needs a technical brief — environment summary, risks, and the objections likely to come up.
category: Sales & Quoting
tools: [search_tickets, search_clients, search_knowledge_base, search_itglue, search_hudu, search_ninjaone_devices]
---

# Presales Technical Brief

Prepare the engineer walking into a sales conversation: what the client's environment actually looks like per our records, the technical risks relevant to what's being proposed, and the objections the room will raise — with honest answers ready.

## When to use

- "I'm the engineer on <client>'s expansion call — brief me."
- "Prep a technical brief for the <client> proposal meeting."
- "What will they push back on when we pitch the migration?"

## Steps

1. Get the context: which client (resolve via `search_clients`), what is being proposed, and when the call is. The proposal shapes what matters in the brief.
2. Build the environment summary from our own records:
   - `search_tickets` for the last 90 days — recurring issues, recent incidents, open problem tickets, sentiment of key contacts.
   - Documentation via `search_itglue` / `search_hudu` / `search_knowledge_base` (whichever this tenant has) — network layout, server roles, key applications, known constraints.
   - `search_ninjaone_devices` if RMM is connected — device counts, OS spread, aging hardware relevant to the pitch.
   State the source and freshness of each fact; mark anything not verifiable in our systems as "per <source>, unverified".
3. Risk assessment scoped to the proposal: what in the current environment complicates it (legacy dependencies, single points of failure, undocumented systems, recent instability), and delivery risks the sales motion shouldn't paper over.
4. Likely objections — derive from evidence, not stereotype: recent P1s ("why buy more from you after last month's outage?" — have the honest answer and remediation ready), open tickets contradicting the pitch, cost sensitivity visible in past scope conversations, and technical objections a sharp client IT contact would raise. For each: the objection, the honest response, and what NOT to claim.
5. Output a one-page brief: environment snapshot / relevant history (incl. anything embarrassing they may bring up) / proposal-specific risks / objections with responses / three things the engineer should verify live in the meeting. Internal only.

## Guardrails

- The brief prepares honesty, not spin: never suggest denying or minimizing incidents the client experienced — the objection-handling answers must survive the client checking their own records.
- Facts about the environment cite where they came from (ticket, doc, RMM); stale documentation is flagged with its last-updated date, not presented as current truth.
- Never state what the current agreement covers or what the new work will cost as fact without the agreement/quote evidence — commercial claims stay with the sales owner.
- Internal document: contains incident history and possibly other-client-adjacent context — never send it to the client, and never include another client's data as comparison material.
- Degradation: if documentation tools or RMM aren't connected for this tenant, build from tickets alone and say the environment picture is partial.
