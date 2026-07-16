---
name: Escalation Advisor
description: Sweep active tickets against L2/L3, management, and project-conversion trigger lists and recommend which should escalate — before they age into problems.
category: Escalation
tools: [search_tickets, list_boards, list_ticket_priorities, add_ticket_note]
---

# Escalation Advisor

Proactive sweep of the active queue that answers "what should have been escalated already?" Each recommendation names the trigger it matched and the evidence, so a lead can act on the list in minutes.

## When to use

- A lead asks "what in the queue should be escalated?" or "review my board for stuck tickets that need a higher tier."
- Periodic queue hygiene: a weekly sweep for tickets quietly aging past the point where L1 should still own them.
- A technician asks "should any of my tickets go to L2?"

## Steps

1. Scope the sweep: which boards (list_boards) and whose tickets. Run **separate searches per signal per board** — a single broad search will hit result caps and hide matches. If any search caps out, disclose it in the output.
2. Test each active ticket against the trigger lists:
   - **L2/L3 triggers:** open beyond the tier's time-box (e.g. >2 business days at L1 with no progress), 3+ distinct troubleshooting attempts without resolution, issue class outside L1 scope (server-down, data loss, security incident, network-wide), repeated bounce-backs from the same fix.
   - **Management triggers:** negative sentiment or explicit dissatisfaction, client mentions of contract/cancellation/lawyer, VIP contact with a stalled ticket, SLA breach already occurred, same client hit by 3+ related open tickets.
   - **Project-conversion triggers:** scope grew past break/fix (multi-device rollouts, migrations), fix requires budget approval or procurement, recurring incident whose real fix is an infrastructure change.
3. For each match, record: ticket, client, age, the specific trigger matched, and the one-line evidence ("opened 6 days ago, 4 attempts documented, last touch 3 days ago").
4. Output a ranked list grouped by recommendation type (L2/L3 / management / project), most urgent first. Include a "borderline" section for near-misses a human should eyeball rather than silently dropping them.
5. Recommend only. If the lead wants to act, hand individual tickets to the Escalation Prep skill (or Management Escalation Brief for management-track items) — do not move tickets, change priorities, or post notes as part of the sweep unless explicitly asked, and then only with per-ticket confirmation.

## Guardrails

- This skill recommends; it does not escalate. No writes without an explicit per-ticket instruction.
- Every recommendation must name its trigger and cite documented evidence — no "this feels stuck."
- Never flag on volume or age alone when the ticket shows an active, documented wait (client or vendor holding the ball is not an L1 stall).
- Disclose result caps; a capped sweep is labeled "partial sweep," never presented as the whole queue.
- Trigger thresholds above are defaults — if the tenant has documented escalation criteria (SOP/KB), those win; say which set you used.

## Unattended (Flows) variant

- Follows the Unattended Output Discipline contract: the entire reply is the plain-text sweep report — matches grouped by recommendation type (L2/L3 / management / project), each line naming the ticket, client, age, the specific trigger matched, and the one-line evidence; the borderline section last. No narration.
- Deterministic inputs from the flow: boards, whose tickets, and the trigger thresholds (the tenant's documented criteria if supplied, defaults otherwise — the report states which set ran).
- No matches and no borderlines → reply exactly `NO ESCALATION CANDIDATES.` Capped searches mark the report `PARTIAL SWEEP`.
- Every recommendation still names its trigger and cites documented evidence — "feels stuck" does not survive the unattended bar either.
- Permitted writes: none. Escalating, re-prioritizing, and posting notes stay attended (hand off to Escalation Prep / Management Escalation Brief).
