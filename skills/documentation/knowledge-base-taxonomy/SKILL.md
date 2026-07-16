---
name: Knowledge Base Taxonomy
description: Design the category and tag structure for a knowledge base so articles are findable — a coherent hierarchy, a controlled tag vocabulary, and naming/placement rules — grounded in what the desk actually documents rather than an idealized tree.
category: Documentation
tools: [search_knowledge_base, search_tickets, search_itglue, search_hudu, notion-search, notion-create-pages, notion-update-page]
---

# Knowledge Base Taxonomy

Designs how the knowledge base is organized so people can find articles: a category hierarchy that matches how the desk works, a controlled tag vocabulary (not free-for-all tags), and the rules for where a new article goes and how it's named. The outcome is the taxonomy design and the placement rules — it does not rewrite existing articles.

## When to use

- "Our KB is a mess — design a category and tag structure that makes articles findable."
- Standing up a new KB and needing its structure before articles pile in.
- Article findability is poor (techs re-solve documented issues; see documentation/kb-coverage-report and documentation/kb-ticket-crosslinker) and the structure is the suspect.
- Tags have sprawled into near-duplicates and need consolidating into a controlled vocabulary.

## Steps

1. Ground the design in reality: `search_knowledge_base` to inventory current categories, tags, and article spread; `search_tickets` to see what the desk actually deals with (the topics that generate volume are the categories that must exist); `search_itglue` / `search_hudu` (where enabled) and `notion-search` (if connected) for how documentation is organized elsewhere so the KB taxonomy aligns rather than conflicts.
2. Design the **category hierarchy** — shallow and intuitive (aim for two levels; deep trees hide articles):
   - Organize by how people search (by product/system, by issue type, by audience — tech-facing vs. end-user-facing), matching the ticket-volume topics from step 1.
   - Every category has a one-line definition of what belongs in it, so placement isn't a guess.
   - Flag current categories that are empty, overlapping, or catch-all ("Misc") for merge or retirement.
3. Design the **controlled tag vocabulary** — a defined, finite set (not open free-text):
   - Distinct axes (e.g. product, vendor, symptom, audience) so tags compose instead of colliding.
   - Consolidate existing near-duplicate tags into canonical terms; list the mapping (old → canonical).
   - State that tags are chosen from the vocabulary, not invented per article — the vocabulary is governed.
4. Write the **placement & naming rules**: how to title articles for search (symptom-first, consistent with documentation/kb-article-draft), which category and which tags a new article gets, and a short decision guide for the ambiguous cases. Include a governance note: who owns the taxonomy and how new categories/tags are added (a controlled change, not ad hoc).
5. Output the taxonomy design (hierarchy + definitions), the tag vocabulary with the consolidation mapping, and the placement/naming rules — plus a migration note listing existing articles/tags that need re-filing. If the KB or its taxonomy reference lives in Notion and the destination is confirmed, create the taxonomy doc with `notion-create-pages` or refresh with `notion-update-page`; otherwise paste-ready blocks. This skill does not re-tag or move existing articles — it produces the design and the migration list for a human to execute.

## Guardrails

- **Grounded in real usage, not an idealized tree.** Categories and tags must reflect what the desk documents and searches for (ticket volume + existing content), not an abstract taxonomy nobody will use.
- Shallow and controlled beats comprehensive — too many categories or an ungoverned tag list is the findability problem, not the fix.
- This skill designs the structure; it does not rewrite, re-tag, move, or delete articles. It outputs the design plus a migration list humans act on.
- Result-cap honesty: KB and ticket searches may be capped — note that the volume analysis is a sample, not a census.
- Docs tools are search-only; writing the taxonomy doc goes to Notion only when the member has connected it and confirmed the destination, otherwise paste-ready output.
- Degradation: without IT Glue/Hudu, design from the Thread KB and ticket history alone and note that documentation elsewhere wasn't factored in.
