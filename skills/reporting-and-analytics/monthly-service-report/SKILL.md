---
name: Monthly Service Report
description: Build the polished client-facing monthly service report for one client — volume, response and resolution performance, highlights, and upcoming work — with all internal jargon stripped.
category: Reporting & Analytics
tools: [search_tickets, search_clients, list_boards]
---

# Monthly Service Report

Produce the monthly report a client actually receives: what happened with their support this month, how fast it was handled, what stood out, and what is coming next. This is the polished client artifact; Client Health Report (account-management) is the account manager's internal working read of the same data — build that first when the AM needs to decide what story to tell.

## When to use

- "Build the monthly report for <client>" / "generate <client>'s service report for June."
- An AM preparing the recurring monthly touchpoint email or portal upload.
- A client asked "what did we even get for our money this month?"

## Steps

1. Confirm the client (search_clients) and the month (default: last full calendar month). Ask whether the client sees ticket numbers in their portal — that decides whether references are allowed at all.
2. Run split searches: tickets opened, tickets resolved, still open at month end, and anything high-urgency or long-running. Record result caps.
3. **Volume & responsiveness** — opened, resolved, net open change, and response/resolution performance versus their agreement targets where known. Prior-month comparison if available; no fabricated baselines.
4. **Highlights** — two to four items a business owner cares about: a major issue resolved, a recurring problem eliminated, a project milestone, a quiet month where nothing broke. Positive but honest — if the month was rough, say what happened and what changed as a result.
5. **Upcoming** — open projects, scheduled maintenance, and recommendations already discussed with the client. Never introduce a brand-new recommendation in a report the client reads before the AM does.
6. **Jargon strip (mandatory pass)** — before output, rewrite anything internal: board names, internal status names, tech nicknames, PSA/RMM product jargon, alert-source names, and internal ticket types all become plain client language ("your support queue", "resolved", "our monitoring"). No internal ticket IDs unless step 1 confirmed the client sees them.
7. Output the report ready to send (title, period, sections above), then a separate **internal footer clearly marked not-for-client**: methodology, searches run, caps hit, and anything the AM should know before sending (e.g., an SLA miss the report words carefully).

## Guardrails

- The jargon strip is the product: assume every sentence will be read by a non-technical business owner. If a term requires MSP context to understand, it does not ship.
- Never present capped searches as exact counts — in a client-facing document, round honestly ("over 150 requests handled") rather than printing a capped number as a total.
- Aggregate, not enumerate: themes and counts, never ticket lists; a single incident may be narrated only as a highlight.
- No names of individual techs or client employees in the client copy unless the AM confirms it is wanted.
- Do not invent SLA targets, agreement terms, or upcoming work — if targets are unknown, report raw speed numbers without a pass/fail framing.
- Keep the internal footer visually separate and labeled so it cannot be pasted to the client by accident.

## Unattended (Flows) variant

- Your entire reply is the report, posted verbatim — no narration, no questions, and no internal footer (the unattended output must be 100% client-safe).
- If the client's agreement targets are unknown, omit the pass/fail column silently rather than guessing.
- If any core search fails, output nothing and stop — a wrong client report is worse than a late one.
