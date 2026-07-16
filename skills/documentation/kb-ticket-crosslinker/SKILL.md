---
name: KB Ticket Crosslinker
description: Given a KB article, search resolved tickets on the same topic and append a "related tickets" note or report so the article links to the real cases behind it — result-cap honest. Answers the commonly requested "connect our docs to the tickets they came from" workflow; runs manually on demand.
category: Documentation
tools: [search_knowledge_base, search_tickets, add_ticket_note]
---

# KB Ticket Crosslinker

A KB article is more trustworthy when it points at the actual resolved tickets it distills. Given an article, find the resolved tickets on the same topic and compile a "related tickets" list — as a report, or appended to a chosen ticket/doc.

## When to use

- "Which resolved tickets match this KB article?"
- "Add a related-tickets list to <article>"
- "Show me the real cases behind this runbook"
- Auditing whether a KB article reflects how issues actually get solved

## Steps

1. Load the source article (search_knowledge_base if given a title/topic rather than the text) and extract its topic signals: the problem, the symptoms/keywords, the product or asset, and the resolution.
2. Search resolved/closed tickets for that topic with search_tickets. Run one search per distinct signal (e.g. the error string, the product name, the symptom) and merge — a single broad query misses matches.
3. Rank the matches by how well they fit the article's topic and resolution, keeping the genuinely related ones. Drop tickets that merely share a keyword but solved a different problem.
4. **Result-cap honesty:** if a search hit the result cap, say so ("matched the first 50 — there are likely more") rather than presenting the list as the complete set.
5. Produce the "related tickets" block: ticket number, client (short), close date, and a 5–8-word note on how it relates. Then either return it as a report, or — if the member wants it attached — preview it and add_ticket_note to the chosen ticket/record on confirm.
6. Optionally flag mismatches worth acting on: article says X but several tickets resolved via Y (the doc may be stale — hand off to the stale-doc skill).

## Guardrails

- Read-first, write-on-confirm. Compiling the list changes nothing; appending a note waits on an explicit go-ahead over the previewed text.
- Only link tickets that genuinely match the article's problem-and-resolution; a shared keyword is not a match.
- Result-cap honesty is mandatory — never imply a capped search is exhaustive.
- No fabrication: every ticket number and date must come from a real result; never invent a "related" ticket.
- Keep any appended note plain-text for PSA compatibility.
- Member-invoked only; no unattended variant (Flows can't schedule a cross-corpus audit).
