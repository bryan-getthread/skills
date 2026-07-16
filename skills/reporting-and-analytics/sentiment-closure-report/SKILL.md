---
name: Sentiment Closure Report
description: Closed tickets in a date range bucketed by sentiment score, per client / tech / period, with the driver messages cited so a low score is explainable — not just a number. Answers the commonly requested "how did customers feel about what we closed" workflow; runs manually on demand (Flows can't schedule it).
category: Reporting & Analytics
tools: [search_tickets, search_clients, search_members]
---

# Sentiment Closure Report

How did clients actually feel about the tickets you closed? Take the closed tickets in a range, bucket them by sentiment, break it down by client / tech / period, and — crucially — cite the messages that drove the low (and high) scores so the report is actionable, not just a gauge.

## When to use

- "Sentiment on everything we closed last month"
- "Which clients / techs had the most negative closes?"
- "Show closure sentiment by week for <client>"
- Prepping a QBR or coaching conversation with evidence, not vibes

## Steps

1. Scope the population: closed tickets within the requested date range. Apply any client/tech filter asked for, resolving names with search_clients / search_members first.
2. Pull the tickets with search_tickets and read each ticket's sentiment score. Bucket them — e.g. Negative / Neutral / Positive (or the desk's score bands) — and count per bucket.
3. Break down by the axis requested: per client, per tech, or per period (week/month). Show counts and the share negative, not just an average that hides a bad tail.
4. **Cite drivers.** For the negative bucket (and a couple of strong positives), quote or paraphrase the specific message(s) that drove the score, with the ticket number — so a reader can see *why*. This is the point of the report.
5. **Response/coverage honesty:** state how many closed tickets actually had a sentiment score. If only a fraction were scored, say so — never present sentiment on 12 tickets as if it characterized 300 closes.
6. Close with the one or two patterns worth acting on (a recurring complaint theme, a client trending down) and offer a drill-down.

## Guardrails

- No fabrication. Every score, count, and quoted driver comes from a real ticket; never invent a sentiment value or a message.
- Small-sample honesty is mandatory — always show the scored-vs-total denominator so a thin sample isn't read as representative.
- Don't average away the tail: a handful of very negative closes matters more than the mean.
- Attribute fairly — a negative sentiment isn't automatically the assigned tech's fault; present it as signal, not a verdict.
- Read-only; this report changes nothing.
- On-demand only. Thread Flows have no schedule/cron, so this can't be a recurring flow — run it manually or from an external scheduler.
