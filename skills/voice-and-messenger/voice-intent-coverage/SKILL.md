---
name: Voice Intent Coverage
description: Map which call reasons Voice AI handles end-to-end versus transfers to a human, mine the transfer reasons, and propose the intent/config candidates that would close the biggest gaps. Use for "what is the Voice AI still punting on" or expanding voice coverage.
category: Voice & Messenger
tools: [search_tickets, list_intents, get_intent, create_intent]
---

# Voice Intent Coverage

The Voice AI's transfer log is a ranked shopping list for its own improvement: every reason it hands a call to a human is a call type it could learn. This skill mines handled-vs-transferred outcomes from call tickets, matches them against configured intents, and proposes the next intents worth building.

## When to use

- "Which call reasons does Voice AI handle vs. transfer?"
- "Mine the transferred calls — what should we teach it next?"
- After a coverage push: did transfers for <call type> actually drop?
- Feeding tuning decisions alongside Voice Call QA Review (quality of what it handles; this skill covers *what it doesn't*).

## Steps

1. Collect voice-sourced tickets in the window (default 30 days) with `search_tickets`, split per board for cap honesty. Separate handled end-to-end vs. transferred/escalated using transcript and recap evidence (transfer statements, "let me connect you", escalation notes). Junk closed by Dead-Air Call Filter is excluded from both denominators.
2. For transferred calls, extract the transfer reason from the transcript: what the caller wanted, and why the AI punted — stated by the AI where possible ("I can't reset passwords"), inferred from context otherwise and labeled as inferred.
3. Cluster transfer reasons into call-reason families (password reset, billing question, new-user request, printer issue, "just wants a human", identification failure...). Count by cluster; note single-client skews — one client's weird workflow shouldn't drive global config.
4. Pull configured intents with `list_intents` / `get_intent` and map clusters to them:
   - **Covered & handled** — intent exists, calls resolve,
   - **Covered but transferring** — intent exists yet calls still transfer (config/quality problem → route the examples to Voice Call QA Review),
   - **Uncovered** — no intent matches (coverage gap).
5. Rank uncovered clusters by volume × feasibility. Feasibility is honest: call types requiring identity verification, judgment, or actions the AI can't take (payments, approvals, physical dispatch decisions) are marked "transfer is correct" — not every transfer is a gap, and "wants a human" is a preference to respect, not a bug.
6. Output: coverage table (handled/transferred split, top transfer clusters with counts and example ticket numbers), the covered-but-transferring list, and the top 2–3 intent candidates — each with proposed name, trigger phrasing drawn from real caller wording, and expected volume. Result-cap and inference caveats stated.
7. Only on explicit confirmation, draft the candidate via `create_intent` — clearly as a draft for admin review, never silently enabled into the live call path.

## Guardrails

- Intent tools are admin-only; without them, deliver the analysis and candidates as a report and say the create step needs an admin token.
- Never create or modify intents without explicit confirmation — a bad intent answers live client calls wrong. Draft-for-review is the ceiling of autonomy here.
- Distinguish "AI couldn't" from "AI shouldn't": the report must not recommend automating call types where a wrong answer is costly (security, billing disputes, cancellations) without flagging that judgment call.
- Counts are evidence-linked: every cluster cites example ticket numbers so a human can audit the clustering.
- Transfer-reason inference is labeled as such; don't present inferred reasons with the confidence of stated ones.
- No caller names in the report; ticket numbers and client names only where needed to show skew.
