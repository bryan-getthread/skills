---
name: Client Acronym Decoder
description: Build a per-client jargon and acronym sheet mined from their own tickets — the internal app names, department shorthand, and site nicknames — so a new technician sounds tenured on their first call with that client.
category: Documentation
tools: [search_tickets, search_clients, search_contacts, search_itglue, search_hudu, search_knowledge_base]
---

# Client Acronym Decoder

Every client has a private language — "the LOB," "upstairs," "the Jensen report," the ERP they only ever call by its acquired-company name. This skill mines one client's ticket history for that vocabulary and produces the decoder sheet that lets any technician speak it back.

## When to use

- "Build the acronym sheet for <client>" / "what do <client>'s people mean by <term>?"
- A new technician is taking over an account (pair with communication/new-technician-introduction).
- A confusing ticket just burned time because the client's shorthand was misread.

## Steps

1. Scope to one <client> and pull their vocabulary evidence: search_tickets over that client's history, reading how *their contacts* phrase requests — the mining target is the client's words in their own messages, not the technicians' words. Sweep for:
   - **Acronyms and initialisms** that aren't standard industry terms — likely internal apps, departments, reports, or processes.
   - **House names for systems:** the application referred to by a nickname, project codename, or a vendor's former brand.
   - **Place and people shorthand:** site nicknames ("the annex," "building two"), role shorthand ("the front desk," "dispatch") — mapped to what/where they actually are, using search_contacts / search_clients records where they resolve.
   - **Recurring request phrases:** "run the sync," "reset the board" — client-specific verbs that map to a known procedure.
2. **Resolve each term from evidence:** the meaning must come from context in the tickets (a thread where the term and its referent both appear), existing docs (search_itglue / search_hudu / search_knowledge_base), or the account team. A term the evidence can't resolve goes in the sheet as **UNRESOLVED — ask the client naturally** ("just to make sure I set this up right — which system do you call <term>?"), never guessed.
3. Build the decoder sheet, one row per term: **Term → What it means → In our terms** (the canonical system/procedure name the desk uses — link to the desk glossary, documentation/glossary-builder, where one exists) → **Evidence** (a ticket number where usage is clear). Group by type: systems, places, people/roles, request phrases.
4. Add the one-paragraph **"how they talk" note** at the top when the evidence supports it: the client's channel habits and tone conventions a new technician should match (e.g. "contacts open tickets in one-line shorthand; they expect the tech to expand it"). Facts observed from tickets only — no personality commentary about individuals.
5. Output the sheet and recommend a storage location with the client's docs (their IT Glue/Hudu section or KB client page) so it's found at handoff time — filing is a human step or a kb-article-draft handoff.

## Guardrails

- **Evidence or UNRESOLVED — never guess.** A decoder sheet with a wrong entry makes the new technician sound *less* tenured; every resolved meaning cites where it was established.
- The sheet describes the client's vocabulary, not the client's people: no assessments of individuals, no quotes that embarrass, nothing the client couldn't read over the technician's shoulder.
- No credentials or sensitive identifiers on the sheet, even if ticket threads contain them — a vocabulary sheet travels loosely by design.
- Result-cap honesty: mining is sampled from recent history; mark the sheet with its coverage window and re-run as the relationship ages.
- This is per-client vocabulary; desk-wide term ambiguity belongs in documentation/glossary-builder — cross-link rather than duplicating entries in both.
- Degradation: without IT Glue/Hudu, resolution relies on tickets + KB + the account team; expect more UNRESOLVED rows and say so.
