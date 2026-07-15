---
name: Channel Volume Report
description: Break down ticket volume and outcomes by intake channel — email, chat, voice, portal — including deflection/zero-touch rates and after-hours patterns. Use for "where do our tickets come from", channel-shift tracking, or staffing the phones vs. the inbox.
category: Voice & Messenger
tools: [search_tickets, list_boards]
---

# Channel Volume Report

Answers the operational question behind channel strategy: how much work arrives on each channel, what happens to it there, and when — so decisions about Voice AI coverage, chat rollout, and phone staffing rest on counted tickets instead of vibes.

## When to use

- "Show me ticket volume by channel for last month."
- "Is chat actually replacing email since the Messenger rollout?" (trend comparison)
- "What share of voice calls resolve without a human?" (deflection/zero-touch by channel)
- "When do calls come in?" — after-hours and hourly patterns for staffing.

## Steps

1. Fix the window and comparison window (default: last 30 days vs. the prior 30) in the desk's timezone.
2. Count with `search_tickets`, split per channel/source per board (`list_boards`) — one broad search will hit caps and silently undercount. Track and report cap exposure per cell; a capped count is reported as "≥ N", never as N.
3. Per channel, gather: tickets created; still open; closed; median-ish age of open items; and where the data supports it, reopen count within the window.
4. Deflection/zero-touch per channel where measurable: voice sessions closed with no human touches (Dead-Air Call Filter closures counted separately as junk, not deflection); chat threads resolved without leaving the conversation; email autoresolves. Only count what ticket evidence shows — if the tenant lacks a marker distinguishing AI-resolved from tech-resolved, say the deflection column is not measurable and skip it rather than approximating.
5. Timing patterns: bucket creation timestamps into business-hours vs. after-hours per channel, and day-of-week where the ask is staffing. Voice after-hours volume cross-references After-Hours Voicemail Digest coverage.
6. Output: a channel table (volume, share, delta vs. prior window, open/closed, deflection where measurable, after-hours share), then 2–4 written findings — only ones the numbers actually support ("chat +40% while email flat", "a third of voice volume lands after hours"). End with the caveat block: windows, caps hit, and what wasn't measurable.

## Guardrails

- Result-cap honesty is the defining risk of this report: totals feed staffing and product decisions. Split searches aggressively, mark every capped cell, and never sum capped numbers into a precise-looking total.
- Channel attribution follows the ticket's source/channel field; tickets with no channel marker go in an explicit "unattributed" row — not silently folded into email.
- Correlation discipline: the report says what shifted, not why. "Chat rose after the rollout" is a finding; "the rollout caused it" is the reader's call.
- Read-only: no tickets are touched.
- Trend claims need same-methodology windows: if last month's number was computed differently (or capped differently), flag the comparison as unreliable.
- Keep the report client-anonymous by default (channel totals); per-client breakdowns only when asked, and then client names only — no contacts.
