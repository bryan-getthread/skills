---
name: Personal Context Scaffold
description: A template for a member's personal-context skill — aliases ("when I say X I mean contact Y"), client segments, and personal definitions ("Issue Ticket = >8 days or >2h logged") that every other conversation should honor.
category: Personal Productivity
tools: []
---

# Personal Context Scaffold

Skills are also memory. This is the canonical template for a *personal context* skill: a member's private vocabulary — aliases, segments, thresholds, defaults — written once so the agent applies it in every conversation without being re-told. Copy the scaffold, fill it in, save it as your own skill.

## When to use

- "Remember that when I say <nickname> I mean <contact> at <client>"
- "Set up my personal context / my definitions"
- A member keeps re-explaining the same shorthand every session
- Onboarding a member to Super Magic: capture their working vocabulary on day one

## Steps

1. Explain the idea in one line: this becomes a standing skill the agent loads for you, so definitions stated once apply always.
2. Walk the member through the scaffold sections below, filling only what they actually use — an empty section is deleted, not padded.
3. Resolve every alias to a real, unambiguous record while building (the right <contact> at the right <client>; the exact board name) — an alias bound to a guess is worse than no alias.
4. Save the result as the member's personal skill (via the in-app skill tools when available; otherwise output the finished skill text for them to save). Confirm what was stored, and remind them it's editable any time ("update my context: …").

### The scaffold

```
# <Member>'s Personal Context

## Aliases — people & clients
- When I say "<nickname>", I mean <contact full name> at <client>.
- "<short name>" = the client <full client name>.

## Board & queue vocabulary
- "my board" = <board name>. "the alert queue" = <board name>.

## Client segments
- "my VIPs" = <client>, <client>, <client> — flag anything from them.
- "the co-managed ones" = <client>, <client> — internal IT gets CC'd.

## Personal definitions & thresholds
- "Issue Ticket" = open >8 days OR >2h time logged.
- "stale" = no activity for 48h (not the desk default of 24h).
- "quick win" = estimated <15 min and client is responsive.

## Defaults & preferences
- Default my time entries to <work type> unless I say otherwise.
- When I say "close it", also send my standard closure line first.
- My hours: <start>–<end> <timezone>; don't plan work outside them.
```

## Guardrails

- Personal context is scoped to its owner: apply one member's aliases and definitions only in that member's conversations, never to teammates.
- Never store credentials, personal data beyond names/roles, or anything client-confidential in a context skill — aliases and definitions only.
- On ambiguity, the live conversation wins: if a stored alias conflicts with what the member clearly means right now, ask rather than silently applying the stored meaning.
- Definitions here override desk defaults only for this member's own views and summaries — they never redefine team-wide reports or SLA math.
- Revisit staleness: if an alias target no longer exists (contact left the client), flag it for update instead of guessing a replacement.
