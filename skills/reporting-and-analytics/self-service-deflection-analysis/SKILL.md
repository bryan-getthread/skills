---
name: Self-Service Deflection Analysis
description: Someone asks which tickets could have been resolved by self-service — a KB article, portal request, or automated path — or wants that analysis as JSON for an integration.
category: Reporting & Analytics
tools: [search_tickets, search_knowledge_base, list_boards]
---

# Self-Service Deflection Analysis

Review open or recent tickets and identify which ones a self-service path could have resolved — matched against the KB that actually exists — with an optional machine-readable JSON output for downstream integrations.

## When to use

- "Which of these tickets could self-service have handled?"
- "How much of last month's volume was deflectable?"
- An external system calls in per-ticket: "return a JSON deflection verdict for this ticket."

## Steps

1. Confirm scope: a single ticket, the current open queue, or a period (default: last 30 days). Pull with split searches per board; disclose caps.
2. For each ticket, classify the deflection potential:
   - **Deflectable now** — an existing KB article or portal path answers it (verify with search_knowledge_base; cite the real article).
   - **Deflectable with new content** — repeatable question, no article exists yet; name the article to write.
   - **Deflectable with automation** — needs an intent/flow or password-reset tool, not just content.
   - **Not deflectable** — genuine hands-on work, judgment, or physical presence required.
3. Aggregate: counts and percentage per category, top deflectable patterns, and the highest-value missing articles (volume-ranked).
4. Default output is the human summary: headline deflectable %, top patterns with one representative ticket each, and the top 3 content/automation moves.
5. **JSON option** — if the requester asks for JSON or the run is an API/flow integration, respond with ONLY a raw JSON object, no prose, e.g.:
   `{"scope": "...", "analyzed": n, "capped": bool, "deflectable_now": n, "deflectable_new_content": n, "deflectable_automation": n, "not_deflectable": n, "tickets": [{"id": "...", "verdict": "deflectable_now", "path": "<kb article title>", "confidence": "high"}]}`

## Guardrails

- Only cite KB articles verified via search_knowledge_base — never invent an article title or link.
- A ticket the user *chose* to call in about may still not be a deflection failure; classify on whether self-service *could* have resolved it, and note that adoption is a separate problem.
- Exclude junk/NOC boards; alert tickets are an automation question, not a self-service one.
- Aggregate in the human output; enumerate per-ticket verdicts only in the JSON payload or when scope is a handful of tickets.
- Disclose result caps — a capped deflectable % is directional, not exact.

## Unattended (Flows) variant

- Your entire reply is consumed by the caller: JSON mode means the reply is exactly one JSON object — no narration, no code fences, no trailing commentary.
- Per-ticket calls with insufficient information return `{"verdict": "unknown", "reason": "..."}` rather than a guess.
