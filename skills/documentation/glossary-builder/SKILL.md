---
name: Glossary Builder
description: Build the desk's shared vocabulary — mine tickets for terms used ambiguously or inconsistently, define each once with evidence, and link the glossary everywhere the terms appear, so "the portal" means one thing to everyone.
category: Documentation
tools: [search_tickets, search_knowledge_base, search_itglue, search_hudu, notion-search, notion-create-pages]
---

# Glossary Builder

Mines the ticket record for the words the desk uses loosely — the three systems all called "the portal," the "sync issue" that means four different failures — and turns them into a single-source glossary: one term, one definition, one canonical name, linked from the docs where the confusion lives.

## When to use

- "Build a glossary for the desk" / "our tickets call the same thing five different names."
- New-hire onboarding keeps stumbling on house vocabulary nobody wrote down.
- KB cleanup revealed the same system named differently across articles.

## Steps

1. **Mine for ambiguity** via search_tickets, hunting two patterns (split searches per signal; note caps):
   - **One word, many referents:** generic terms ("the portal," "the server," "the sync," "the VPN," "admin account") — sample recent tickets using each and check whether the referent is consistent. Inconsistent referent → glossary candidate.
   - **Many words, one referent:** the same system/procedure appearing under multiple names across tickets (product name vs house nickname vs the vendor's old brand name). Every alias set → glossary candidate.
   Also sweep search_knowledge_base and search_itglue / search_hudu for the same term collisions inside existing docs.
2. **Confirm the candidate list with the requester** before defining — the desk knows which ambiguities actually bite. Rank by evidence: terms implicated in mis-routed or slow tickets first (cite ticket numbers).
3. **Define each term once**, entry format:
   - **Term** — the canonical name the desk agrees to use going forward.
   - **Definition** — one or two plain sentences a first-week technician understands.
   - **Aliases** — the other names seen in the wild, listed so search finds the entry ("also seen as: …"). Aliases are acknowledged, not endorsed.
   - **Not to be confused with** — the near-miss term, when the mining found actual confusion between two.
   - **Evidence** — optionally, a ticket number showing the confusion this entry resolves.
   Definitions come from the requester/desk or from documentation — never invented; a candidate nobody can define gets listed as an open question, not guessed at.
4. **Publish once, link everywhere:** the glossary lives in exactly one place — the desk's KB or, if the member has connected Notion and prefers it there, a page via notion-create-pages. Then produce the **link list**: the KB articles, runbooks, and doc entries found in step 1 that use ambiguous or alias terms, each with the recommended edit ("uses 'the portal' — link to glossary / replace with <canonical term>"). Editing those docs is a human follow-up (or kb-article-draft per article), not a bulk write.
5. Output: the glossary entries, the open-question terms, the link list, and a one-line adoption note for the desk ("new tickets and docs use canonical names; aliases stay searchable via the glossary").

## Guardrails

- **Define once:** one glossary, one location. A second copy is the seed of the next inconsistency — if a team wants it elsewhere too, link, don't duplicate.
- Never invent a definition. Terms are defined by the desk's confirmed usage or existing documentation; ambiguity that survives mining goes in as an open question for a human.
- Canonical-name choices are recommendations until the requester confirms — renaming what the desk calls things is a team decision.
- Sanitized entries: define systems and roles generically; no client names, no people, no credentials, no environment-specific identifiers in a glossary that may be shared desk-wide.
- Result-cap honesty: term mining over tickets is sampled, not exhaustive — say so; the glossary grows by iteration, and this skill can be re-run.
- Keep definitions translatable: short sentences, no idioms — desks run in more than one language. For per-client (rather than desk-wide) vocabulary, use documentation/client-acronym-decoder instead.
- Degradation: notion-create-pages requires the member's Notion connection; otherwise publish to the KB via a kb-article-draft handoff. Without IT Glue/Hudu the doc sweep covers KB + tickets only.
