---
name: Status Check Intent Design
description: Build the "any update on my ticket?" intent — answer from the ticket's real status and last client-visible update instead of interrupting the tech, and only escalate when the trail has genuinely gone cold. Use when asked to "build a status update intent", "stop status-check pings", or when update-requests rank high in Intent Mining.
category: Automation & Flows
tools: [list_intents, get_intent, create_intent, update_intent, set_variation_arguments, set_variation_replies, update_variation, search_tickets]
---

# Status Check Intent Design

Guide the admin through building a status-check intent that turns "any update?" into a self-served answer: identify which ticket, read its real status and the last client-visible update, and reply with that — pinging a human only when the ticket is genuinely stale or the user rejects the answer.

## When to use

- "Build an intent for ticket status questions."
- "Techs get interrupted all day by 'any update?' messages."
- Intent Mining shows update-requests as one of the biggest non-issue conversation types.

## Design

**Trigger phrases (adapt to real ticket language):** "any update on my ticket", "status of my ticket", "any news on", "has anyone looked at my request", "is my ticket being worked on", "following up on my ticket", "checking in on the issue I reported", "when will my ticket be fixed", "still waiting on", "ticket number <ticket> update".
Near-miss watch: "any update?" inside an active ticket thread is conversation, not a new intent match; a follow-up that adds new symptoms should append to the ticket, not just report status.

**Arguments to collect (identification):**
- Ticket number if they have it; otherwise enough to find it — roughly what it was about and when reported.
- If the requester has multiple open tickets and gave no number → list their open tickets briefly and ask which.
- Only the requester's own tickets (or their client's, per the client's visibility policy) — never resolve a status request against another contact's ticket.

**Reply flow (answer from the record):**
1. Locate the ticket (the runtime equivalent of `search_tickets` scoped to the requester/company).
2. Reply with: current status (translated to plain language — "Scheduled", not an internal status code), the last client-visible update and its date, and the next expected step if one is recorded (scheduled visit, waiting on a part, waiting on the user).
3. **Client-visible only:** never surface internal notes, tech names in blame-able contexts, or internal discussions. If the last client-visible update is genuinely the answer, that is the answer.
4. If the ticket is waiting on the user → say so plainly and restate what is needed (this alone resolves a chunk of "any update?" traffic).
5. **Staleness branch:** if there has been no client-visible activity beyond the desk's freshness threshold (admin-set, e.g. several business days), do not just read back the silence — add a note to the ticket flagging the client asked for an update and the ticket appears stale, and tell the user the desk has been nudged.
6. If the user pushes back or expresses escalation-level frustration → route to the human escalation path with the conversation attached; never argue the status.

**Handoff rule:** the intent reports and nudges; it never changes priority, promises resolution dates, or reopens/closes tickets. Frustrated users get a human.

**Variation hooks (per client):** status-name translations, the freshness threshold, whether contacts may see all their company's tickets or only their own, escalation contact/path.

**Success metric:** deflection rate on status conversations (answered without a human touch) and reduction in tech interruptions; counter-metric: escalations that started as status checks (rising = the answers aren't landing, or the desk really is going quiet — both worth knowing).

## Steps

1. `list_intents` — check for an existing status/update intent; prefer updating.
2. `search_tickets` for recent "any update"-type notes and tickets; mine phrasing for triggers and measure how often the honest answer would have been "waiting on you" vs. "genuinely stale" (calibrates the freshness threshold with real data).
3. Agree the freshness threshold and visibility policy with the admin — these two decisions define the intent's behavior.
4. Draft the full spec: triggers, identification arguments, answer template, staleness branch, escalation branch, variations. Test plan: 5 should-match, 3–5 should-not (in-thread follow-up and new-symptom near-misses).
5. Show the draft; on explicit confirmation call `create_intent` then the variation tools.
6. Report what was created, restate the test plan, recommend activation after tests pass. Do not activate.

## Guardrails

- Intent tools are admin-only; if absent, output the complete written spec for an admin to apply.
- Client-visible information only — internal notes, private discussions, and internal status jargon never appear in replies.
- Strict requester scoping: never return status for tickets the asking contact is not entitled to see; when identification is ambiguous, ask, don't guess.
- No promises: no resolution dates, no "shortly", no priority changes. The staleness nudge is a plain-text note on the ticket, not a priority escalation.
- Confirm before any write; never activate on the member's behalf.
