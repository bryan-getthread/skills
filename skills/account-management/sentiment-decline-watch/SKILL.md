---
name: Sentiment Decline Watch
description: Find clients whose sentiment is trending down, show the evidence and the specific conversations driving it, and draft suggested outreach for each.
category: Account Management
tools: [search_tickets, search_clients, search_contacts]
---

# Sentiment Decline Watch

Catch relationship damage while it is still one bad thread, not a churn conversation. Focused purely on the sentiment signal — for the full four-signal portfolio view, use Client Risk Scan.

## When to use

- "Which clients are getting unhappy with us?"
- "Show me sentiment trends across accounts this month."
- "Why is <client>'s sentiment dropping, and what should I do about it?"

## Steps

1. Confirm scope (whole portfolio or a named client) and window. Default to the last 30 days versus the prior 30.
2. With search_tickets, find clients whose sentiment scores trend down across the two windows. For a single named client, pull that client's scored threads directly.
3. For each declining client, present:
   - **Evidence:** the trend in one line (direction, roughly how sharp, over what period). If searches hit a result cap, say the trend is based on a partial sample.
   - **At-risk conversations:** the two or three open threads doing the damage — for each, who the contact is, what they are upset about in plain language, and the current state. One representative example per underlying issue; no ticket IDs in any text that could be forwarded.
   - **Suggested outreach:** a two-to-four-sentence draft the account manager could send or say — acknowledging the specific friction, stating what is being done, and offering a call. Clearly labeled DRAFT.
4. Order clients by steepness of decline, worst first.
5. If nothing is declining, say so plainly and stop — do not manufacture concern.

## Guardrails

- One low score is a data point, not a trend. Require multiple scored interactions before naming a client as declining, and say how many the trend rests on.
- Sentiment scores describe conversations, not people. Never attribute the decline to a named technician's performance — describe the interaction pattern; performance conversations belong to the manager.
- Outreach drafts are suggestions for the account manager, not messages to send. Never send outreach yourself.
- Internal analysis and client-facing draft are clearly separated in the output; the draft contains no ticket IDs and no internal commentary.
- Do not speculate about causes the threads do not support.
