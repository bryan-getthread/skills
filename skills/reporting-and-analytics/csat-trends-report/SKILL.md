---
name: CSAT Trends Report
description: Someone wants satisfaction scores over time — by client, technician, or ticket category — with honest treatment of response rates, so a 3-response month is never presented like a 300-response month.
category: Reporting & Analytics
tools: [search_tickets, search_clients, search_members, list_boards]
---

# CSAT Trends Report

Chart survey/sentiment scores across a period and slice them by client, tech, or category — with response-rate honesty as the organizing principle. CSAT's dirty secret is that most tickets never get scored; this report always shows the N next to the score and says plainly when the N is too small to mean anything.

## When to use

- "How's our CSAT trending?" / "satisfaction scores for the last six months."
- "Which clients are unhappy?" or "which techs get the best scores?"
- Before a QBR or a comp conversation where someone is about to quote a CSAT number.

## Steps

1. Confirm the period (default: last 6 months, monthly buckets) and the slice requested (overall, by client, by tech, by category). Establish what satisfaction signal exists for this desk — explicit survey scores, thumbs ratings, or sentiment analysis on ticket threads — and label the output with which one is being used; they are not interchangeable.
2. Run split searches per bucket: total resolved tickets, tickets with a satisfaction response, and the response values. Record result caps.
3. Compute per bucket: score, response count (N), and response rate (N / resolved). **Every score is displayed with its N.** Apply confidence labeling: under ~10 responses = "anecdote — do not act on this number alone"; 10–30 = "directional"; 30+ = "usable". Do not average away the problem by merging months to hide a thin N without saying so.
4. **Trend read** — is the score moving, or is the response *rate* moving? A falling response rate with a stable score often means only angry people still answer (or only happy ones); check whether the responding population changed before declaring a satisfaction change.
5. **Slice** — per client / tech / category tables, sorted by score, but only for slices clearing the directional threshold; lump the rest into "insufficient responses" rather than printing misleading rows.
6. For any genuinely declining slice with adequate N, pull 3–5 negative responses and summarize the recurring theme (aggregate the complaint, don't quote clients verbatim into a report others will circulate).
7. Output: trend table (bucket / score / N / response rate / confidence), slice tables, one paragraph on the biggest real movement, and a methodology note — signal type, period, thresholds used, searches run, caps hit.

## Guardrails

- Low N is said plainly, every time, next to the number — never in a footnote. This is the skill's reason to exist.
- Per-tech CSAT is role- and mix-sensitive: techs handling escalations and outages meet angrier people. Benchmark role-aware, never rank on CSAT alone, and never volume alone either.
- Sentiment analysis scores and explicit survey scores must never be blended into one trend line; if both exist, show two labeled lines.
- Never present capped searches as exact totals; disclose caps.
- Aggregate, not enumerate — themes from negative feedback, not a wall of quotes; name a specific ticket only when a single incident explains a score cliff.
- Cross-reference Sentiment Decline Watch (account-management) when the request is really "alert me when a client turns sour" rather than a periodic report.
