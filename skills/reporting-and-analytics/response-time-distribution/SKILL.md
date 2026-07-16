---
name: Response Time Distribution
description: Someone wants the shape of first-response times, not just the average — p50/p90 and the long tail that the mean hides.
category: Reporting & Analytics
tools: [search_tickets, list_boards, list_ticket_priorities, list_ticket_statuses]
---

# Response Time Distribution

Report the distribution of first-response times — median, p90, and the tail — instead of a single average, because the mean hides both the typical experience and the worst-case clients feel. Answers "how fast do we usually respond, and how bad does it get?"

## When to use

- "What's our first-response time?" — and you want an honest answer, not a flattering mean.
- A few very slow tickets are dragging the average and someone wants the real shape.
- Setting or sanity-checking an SLA target against what the desk actually delivers.

## Steps

1. Confirm the period, boards in scope (list_boards), and the definition of "first response" for this tenant — first outbound member reply to the contact, excluding automated acknowledgements and internal notes. State it; auto-acks make response time look artificially perfect.
2. Run split searches per board and per priority tier (result-cap honesty: many small searches; record caps). Capture the response timestamp source you are using.
3. Report percentiles, not just the mean: p50 (median), p90, and p95 or max for the tail, alongside the average — and explain the gap between mean and median when it is large (right-skew from a few slow tickets).
4. Show the distribution shape compactly (a text histogram of response-time buckets) so the reader sees the cluster and the tail, not just point statistics.
5. Slice by priority — a healthy p90 overall can still hide slow critical-ticket responses. Segment by business-hours vs after-hours arrival if that split matters for the desk.
6. Output: the percentile table (with denominators), the bucket histogram, the priority slices, an explicit note on skew, and a methodology note (first-response definition, exclusions, boards, period, caps).

## Guardrails

- Lead with median and p90; never report the mean alone — a single average is the metric this skill exists to replace.
- State the first-response definition and exclude automated acknowledgements, or the numbers flatter the desk.
- Show denominators with every percentile; a p90 off a handful of tickets is unstable — caveat it.
- Distinguish business-hours from after-hours arrivals where relevant; a slow tail may be entirely overnight intake, which is a staffing question, not a responsiveness failure.
- Never present capped searches as complete — disclose caps, which truncate the tail exactly where it matters.
- Do not invent timestamps; exclude tickets with unreliable response data and say how many.
