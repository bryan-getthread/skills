---
name: Typosquat Digest Triage
description: A dnstwister-style lookalike-domain report arrived with dozens of permutation candidates — score every candidate for attack capability, discard parked/inactive noise, and open tickets only for the capable or active lookalikes, each handed to the typosquat-domain-alert workup.
category: Security
tools: [search_tickets, search_clients, liongard_domain, web_search, create_ticket, add_ticket_note]
---

# Typosquat Digest Triage

A permutation-monitoring digest (dnstwister, dnstwist output, or a vendor's lookalike report) lists every registered domain within edit distance of a client's — most of which are unrelated words, ancient parked domains, or dead registrations. This skill is the batch filter: score everything, ticket only what can actually hurt, and route each real one to the single-domain workup. The digest must not over-ticket.

## When to use

- A scheduled lookalike-domain monitoring report lands as a ticket or is pasted in with many candidate domains.
- "Go through this dnstwister report for <client> and tell me which ones matter."
- A vendor's brand-protection digest needs triage before anyone works it.

## Steps

1. Extract the full candidate list from the digest exactly as written — every domain, no truncation. If the report itself notes it is capped or partial, carry that caveat into the output.
2. Score EVERY candidate on capability and intent signals, using passive sources only (`liongard_domain` where enabled for DNS/registrar visibility, passive `web_search` for registration facts — never visit any candidate domain):
   - MX records present → can send/receive mail (email-attack capable): strong signal.
   - A/AAAA record pointing at live hosting → can serve a credential-harvest page: strong signal.
   - Recently registered (days–weeks) → strong signal.
   - Visually close to the client's domain (homoglyph, single-character swap, added hyphen/word like "-support" or "-billing") → raises severity; a distant permutation that happens to be a real unrelated word lowers it.
   - Parked page, registered years ago, no MX, no meaningful A record, or clearly an unrelated legitimate business → noise.
3. Check for active use: `search_tickets` for any candidate already appearing in the client's mail, phishing reports, or fraud tickets. Any hit → that candidate is ACTIVE, top severity.
4. Bucket the candidates:
   - ACTIVE (seen in the wild) or CAPABLE (MX and/or live hosting, especially if recent + visually close) → ticket-worthy.
   - WATCH (visually close but currently inert — old, parked, no MX) → listed in the summary for the watchlist, no ticket.
   - NOISE (unrelated word, dead, distant permutation) → discarded with a one-line reason, no ticket.
5. For each ticket-worthy candidate, `create_ticket` — one ticket per domain, not one bulk ticket — titled to a standard form (e.g. "Lookalike domain workup: <domain> vs <client-domain>") containing the digest source, the scored signals with as-of dates, and the instruction to run the typosquat-domain-alert workup on it. That skill owns the deep workup and the client warning; this skill does not draft client communications.
6. Post the triage summary (plain-text note on the digest ticket via `add_ticket_note`, or reply in chat): total candidates, counts per bucket, the ticketed domains with real ticket numbers, the watchlist, and the discard list with reasons. Every candidate from the report is accounted for in exactly one bucket.

## Guardrails

- Never visit, resolve links on, screenshot, or otherwise interact with any candidate domain — passive sources only, same rule as typosquat-domain-alert.
- The digest must not over-ticket: parked/inactive/unrelated candidates get zero tickets. One ticket per capable domain, never a bulk "37 lookalikes" ticket that nobody can work.
- Never discard silently: every candidate appears in the summary with its bucket and reason, so a human can override a NOISE call.
- Score all candidates before ticketing any — capability is relative, and the worst offenders should be visible as such in the summary.
- Do not invent ticket numbers; list only tickets actually created.
- Active-use hits escalate beyond this skill: an ACTIVE candidate with live phishing threads belongs in the phishing/BEC response path immediately, not just a workup queue — say so in its ticket.
- Degradation: Liongard absent → DNS visibility comes from passive web_search only; if MX/A presence cannot be determined for a visually-close candidate, bucket it WATCH (not NOISE) and state the visibility gap.
