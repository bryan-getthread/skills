---
name: Prospect Research Brief
description: When sales needs a pre-sales brief on a prospect built from public information — likely stack, company size, and probable pain points — before a first call.
category: Sales & Quoting
tools: [web_search]
---

# Prospect Research Brief

Build a one-page pre-sales brief on a prospect from public sources only: what they do, roughly how big they are, technology hints visible from the outside, and the pains an MSP conversation should probe.

## When to use

- "First call with <prospect> tomorrow — research them."
- "What can we learn about <prospect> before the discovery meeting?"
- "Build a pre-sales brief on this company."

## Steps

1. Confirm the target unambiguously: company name plus a disambiguator (website, city/region) — common names collide, and a brief on the wrong company is worse than none.
2. Research with `web_search`, in passes:
   - **Basics** — what they do, industry, locations, headcount signal (site, hiring pages, public registries/profiles).
   - **Stack hints** — job postings (the richest public source: tools named in IT/ops roles), website tech clues, published case studies or vendor references, public breach/incident coverage if any.
   - **Trajectory** — recent news: growth, new sites, leadership changes, funding, M&A — each one a reason they might be rethinking IT.
   - **Compliance context** — industry-implied obligations (health, finance, legal, government supply chains) worth probing.
3. Convert findings to likely pains, each tied to its evidence: e.g. multi-site + no IT hires visible → stretched or absent internal IT; hiring for a role that lists legacy tools → modernization pressure; regulated industry + small size → compliance burden without dedicated staff.
4. Draft 5–8 discovery questions the seller can actually ask, ordered from safe openers to the probing ones, each linked to a finding.
5. Output the brief: company snapshot / stack hints / trajectory / likely pains / discovery questions / sources list with dates. Keep it to one page — a brief the seller won't read helps nobody.

## Guardrails

- Public information only, via `web_search`. No scraping behind logins, no pretexting, no using another client's private data as "intel" — ever.
- Separate fact from inference typographically: findings cite a source; inferences are labeled "likely/possibly" with the reasoning. Never present a stack guess as confirmed.
- No personal profiling of individuals beyond public professional roles (title, public statements). No personal contact details in the brief.
- If the prospect turns out to be an existing client's subsidiary, competitor-sensitive relation, or otherwise conflicted, flag it and stop rather than deepening the research.
- This is an internal document — never send it or its contents to the prospect, and don't contact the prospect in any way.
