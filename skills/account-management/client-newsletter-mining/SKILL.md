---
name: Client Newsletter Mining
description: Mine recent ticket patterns across the client base for newsletter and awareness topics — what users are struggling with, what threats are landing, what tips would cut ticket volume.
category: Account Management
tools: [search_tickets, search_knowledge_base, web_search]
---

# Client Newsletter Mining

Turn the ticket queue into an editorial calendar: the issues real users hit this month are the topics they will actually read about. Produces topic candidates with draft blurbs, not a finished newsletter.

## When to use

- "What should go in this month's client newsletter?"
- "Mine recent tickets for awareness topics."
- "Give me tips-and-tricks content ideas from what users are asking."

## Steps

1. Confirm the window (default: the last 30 days) and scope (default: the whole client base — this is cross-client pattern mining, not a single account).
2. With search_tickets, sweep for content-worthy patterns across clients:
   - **How-do-I clusters** — the same user question recurring → a tips article that deflects tickets.
   - **Threat activity** — phishing waves, scam patterns, MFA-fatigue attempts observed across clients → an awareness piece.
   - **Change-driven confusion** — a vendor UI change or update generating tickets → a "what changed and what to do" explainer.
   - **Seasonal/upcoming** — end-of-support dates, renewal seasons, travel-security timing.
3. Cross-check candidate topics against search_knowledge_base — an existing KB article can be repurposed as newsletter copy and linked.
4. Optionally use web_search to confirm public details (a vendor's announced change, an end-of-support date) before citing them. Never cite unverified specifics.
5. For each of the top five to eight topics, output: a working headline, a two-or-three-sentence draft blurb written for end users, the evidence (how often it appeared, across roughly how many clients), and a suggested call to action.
6. Rank by breadth of relevance across the client base.

## Guardrails

- Newsletter content is client-facing by definition: absolutely no client names, user names, ticket IDs, or details that could identify whose incident inspired a topic. Every example is fully generalized.
- Threat-related items use defensive writing — describe the pattern and the protective action; never state or imply that a specific client was breached or compromised.
- One representative (anonymized) pattern per topic; do not stack incident details.
- Verify externally-checkable facts (dates, vendor changes) with web_search or drop them; never fabricate statistics about "rising attacks."
- If searches hit result caps, frequency claims are phrased loosely ("we saw this repeatedly this month"), never as counts.
- Output is topic candidates and blurbs for a human editor — do not send or publish anything.
