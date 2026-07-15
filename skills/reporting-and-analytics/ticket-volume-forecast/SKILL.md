---
name: Ticket Volume Forecast
description: Someone asks how many tickets to expect in the coming weeks, or wants an average-daily-volume forecast from recent history for planning.
category: Reporting & Analytics
tools: [search_tickets, list_boards]
---

# Ticket Volume Forecast

Forecast average daily ticket volume for the coming weeks from recent history — with the uncertainty stated plainly, because a service desk forecast from a few weeks of counts is an estimate, not a promise.

## When to use

- "What ticket volume should we expect over the next month?"
- "Forecast daily volume so I can plan coverage / justify a hire."
- "Is our volume trending up or down?"

## Steps

1. Confirm the horizon (default: next 4 weeks) and pull daily created-ticket counts for at least the trailing 8–12 weeks, using split searches per board per week to stay under result caps; disclose caps.
2. Exclude junk/NOC boards and known one-off spikes (a major incident, an onboarding wave, an alert storm) from the baseline — but report them, since spikes recur.
3. Compute the baseline: average daily volume by weekday (weekday patterns matter more than a flat mean), and the week-over-week trend.
4. Produce the forecast: expected daily average per weekday and weekly total for the horizon, **as a range** (e.g., "45–60/day midweek"), with the trend applied only if it is consistent across several weeks.
5. State uncertainty honestly and specifically: how many weeks of history back this, what was excluded, what would break the forecast (new client onboarding, seasonal patterns longer than the history window, monitoring changes).
6. Close with what the range implies for the asker's decision (coverage, hiring) framed as "if the pattern holds."

## Guardrails

- Never present a point forecast without a range, and never imply statistical rigor the data cannot support — this is trend extrapolation, not a model.
- If history is shorter than ~6 weeks or heavily distorted by one-offs, say the forecast is low-confidence and recommend revisiting after more history accrues.
- Do not annualize or extrapolate beyond ~6–8 weeks; refuse politely and explain why.
- Disclose every result cap — an undercounted history silently deflates the whole forecast.
- Volume forecasts inform staffing conversations; they are not, alone, a case for cutting or adding heads — say so when the question is a staffing one.
