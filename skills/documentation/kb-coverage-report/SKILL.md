---
name: KB Coverage Report
description: Compare the desk's top ticket intents against existing KB coverage and return a ranked list of the articles most worth writing next.
category: Documentation
tools: [search_tickets, search_knowledge_base, search_itglue, search_hudu]
---

# KB Coverage Report

Measure where ticket demand and knowledge-base supply diverge, and turn the gap into a
prioritized writing backlog.

## When to use

- "What KB articles should we write next?"
- "How good is our KB coverage for what we actually get tickets about?"
- Planning a documentation sprint or onboarding a documentation owner.
- Deflection push: which self-service articles would remove the most tickets?

## Steps

1. Build the demand side: pull tickets for the window (default: last 90 days) with `search_tickets` and group into intent themes (password/MFA resets, printer issues, VPN, email delivery, onboarding requests, specific line-of-business apps...). Run **separate searches per theme** rather than one broad pull; count per theme and per <client> spread.
2. Build the supply side: for each top theme (default: top 15), search `search_knowledge_base` (plus `search_itglue` / `search_hudu` where enabled) and grade coverage:
   - **Covered** — a current article squarely addresses the theme.
   - **Partial** — related article exists but misses the common variant techs actually hit.
   - **Stale** — article exists but tickets contradict it (hand to stale-doc-hygiene).
   - **Missing** — nothing found.
3. Rank the gaps by expected payoff: ticket volume × average handling effort × breadth across clients. Themes resolvable by end users get a deflection bonus; themes always escalated get a training bonus.
4. Output the report: a coverage table (theme, ticket count, coverage grade, evidence article/ticket references) followed by the **ranked write list** — for each entry, the proposed article title, the audience (tech-facing vs end-user self-service), and 2–3 example tickets to source it from.
5. Offer to draft the top entries with kb-article-draft or sop-builder.

## Guardrails

- **Result-cap honesty:** ticket counts from capped searches are reported as "at least N"; say which themes were measured on partial data.
- Intent grouping is judgment — show example ticket numbers per theme so a human can sanity-check the clustering; never inflate counts by double-counting a ticket across themes.
- Cite only articles and tickets that were actually returned; no invented titles or links.
- Read-only: this skill reports; drafting and publishing are separate, human-gated steps.
- If external doc platforms are not enabled, grade coverage on the Thread KB alone and say so — the gap list may overstate misses.
