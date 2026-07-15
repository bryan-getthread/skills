---
name: My Queue Summary
description: A technician asks what is on their plate, what needs replies, what is urgent, or what to work next — summarize their assigned tickets with a clear next step for each.
category: Reporting & Analytics
tools: [search_tickets, search_members]
---

# My Queue Summary

Give an individual technician a prioritized read of their own open tickets — bucketed into needs-replies, urgent, and everything else — with one clear next step per ticket.

## When to use

- "Give me a summary of my open tickets — what needs replies, anything urgent."
- "What should I work on today?" / "What's next?"
- "Prioritize my queue" or a morning/EOD personal digest.
- The technician asks for a short version: "keep it to 3 lines."

## Steps

1. Identify the requesting technician (search_members if needed) and pull their open assigned tickets with search_tickets. If the count may exceed the result cap, split the search per board or per status and disclose it.
2. Bucket the results:
   - **Needs replies** — the last message is from the client and is awaiting the tech.
   - **Urgent** — high priority, imminent SLA target, or negative-sentiment thread.
   - **Everything else** — ranked by priority, SLA risk, then age.
3. For each ticket give a one-line state and a concrete next step ("reply with the workaround", "chase the vendor", "schedule the visit"), not just a status.
4. Lead with the single most important ticket to work next and why.
5. If the tech asked for a digest ("3 lines", "short version"), compress to: needs-replies count + the top one, urgent count + the top one, and the recommended next ticket. Nothing else.

## Guardrails

- Scope strictly to the requesting technician's own tickets — never another member's queue.
- Lead with the top priority; do not dump a flat unranked list.
- Exclude auto-closed statuses and junk/NOC noise boards from the buckets.
- If any search hit a result cap, say so ("showing the first N by priority") rather than presenting the list as complete.
- Do not invent ticket numbers or states; every line must come from a returned ticket.

## Unattended (Flows) variant

- Your entire reply is delivered verbatim as the digest — no narration, no preamble.
- Use the 3-line digest format by default.
- If the search returns zero open tickets, reply with a single line saying the queue is clear; when in doubt, output nothing.

## Consolidates

My assigned tickets summary, technician ticket review, priority-of-the-day, needs-replies digest, 3-line morning digest, and "what should I work next" skills.
