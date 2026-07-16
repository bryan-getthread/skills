---
name: SOP Candidate Finder
description: Sweep recently resolved tickets for documentation-worthy resolutions — recurring fixes, senior-only knowledge, vendor workarounds — and rank what should become an SOP or KB article.
category: Documentation
tools: [search_tickets, search_knowledge_base, search_itglue, search_hudu]
---

# SOP Candidate Finder

Mine resolved tickets for the fixes worth writing down, check whether documentation already
exists, and return a ranked candidate list ready to feed into sop-builder or kb-article-draft.

## When to use

- "What should we document from last month's tickets?"
- "Find resolutions we keep re-solving from scratch."
- A documentation push or slow week needs a prioritized writing backlog.
- A lead wants to capture a departing senior tech's knowledge.

## Steps

1. Pull resolved/closed tickets for the requested window (default: last 30 days) with `search_tickets`. Run **separate searches per signal** rather than one broad pull:
   - **Recurrence** — same symptom/category resolved 3+ times (across clients or repeatedly for one <client>).
   - **Senior-only knowledge** — resolutions that always land on the same senior member or required escalation to resolve.
   - **Vendor workarounds** — resolutions referencing a vendor case, hotfix, or non-obvious workaround.
   - **High effort** — resolutions with large logged time that a procedure could compress.
2. For each candidate, check `search_knowledge_base` (plus `search_itglue` / `search_hudu` where enabled) for existing coverage. Drop candidates that are already well documented; flag ones covered by a stale or partial doc as "update" rather than "new".
3. Rank remaining candidates by expected payoff: frequency × effort per occurrence × breadth (multi-client beats single-client). Senior-only knowledge gets a retention bump regardless of frequency.
4. Output a ranked table: candidate topic, evidence (ticket numbers and counts), signal type, existing-doc status (none / stale / partial), and suggested next step (sop-builder for procedures, kb-article-draft for fix write-ups).
5. Offer to draft the top candidate.

## Guardrails

- **Result-cap honesty:** if any search hits its result cap, say counts are "at least N" — never present a capped count as exact.
- Cite real ticket numbers for every candidate; do not invent tickets, counts, or documentation links.
- Recurrence claims need matching symptoms/resolutions, not just similar wording in titles.
- Read-only: this skill searches and reports; it never creates docs or modifies tickets.
- If `search_itglue` / `search_hudu` are not enabled, note that existing-coverage checks used the Thread KB only.

## Unattended (Flows) variant

- Follows the Unattended Output Discipline contract: the entire reply is the ranked candidate table in plain text — topic, evidence ticket numbers and counts, signal type, existing-doc status, suggested next step. No narration, no offer to draft.
- Deterministic inputs from the flow: the window and the boards to sweep. Capped searches are labeled "at least N" inside the table; doc platforms not enabled are named as unchecked.
- No documentation-worthy candidates → reply exactly `NO SOP CANDIDATES.` — a scheduled run never pads the list to look productive.
- Every cited ticket number must come from the searches; recurrence claims still require matching symptoms and resolutions, not title similarity.
- Permitted writes: none. Drafting (sop-builder / kb-article-draft) and any ticket writes stay attended.
