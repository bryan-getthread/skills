---
name: First Contact Resolution Report
description: Someone wants the desk's FCR rate — what share of tickets were resolved by the first assignee with no handoffs — with the definition stated explicitly in the output so the number can't be argued with later.
category: Reporting & Analytics
tools: [search_tickets, search_members, list_boards, list_ticket_statuses]
---

# First Contact Resolution Report

Measure first-contact resolution for the desk (or a board, team, or tech) over a period, using one explicit definition printed in the output: a ticket counts as FCR when it was resolved directly by its first assignee with no intermediate handoffs — no reassignment, no board bounce, no escalation. FCR debates are almost always definition debates; this skill ends them by shipping the definition with the number.

## When to use

- "What's our FCR rate?" / "how often do we fix things on the first touch?"
- A manager benchmarking the desk before or after a triage/automation change.
- "Why do our tickets bounce between people so much?" — the inverse question.

## Steps

1. Confirm the period (default: last full month) and scope — whole desk, a board (list_boards), a team, or one tech (search_members).
2. State the definition up front and get agreement if the requester implies a different one (some desks count "resolved within one business day" as FCR — that is a different metric; offer to run both, labeled separately).
3. Run split searches per board: tickets resolved in the period, then identify the non-FCR set — tickets showing reassignment, board moves, or escalation between first assignment and resolution. Use the strongest signals the data exposes; where handoffs are only partially visible, say which signal you used and what it can miss.
4. Compute FCR = (resolved with no handoffs) / (all resolved), overall and broken down by board and by ticket category where volume permits. Exclude auto-closed/no-touch tickets from both numerator and denominator — automation resolutions are a different (good) story and inflate FCR if mixed in.
5. **Anti-pattern check** — a suspiciously high FCR can mean techs avoid escalating when they should; scan the non-FCR set's reopen rate vs the FCR set's. If FCR tickets reopen more, flag it: speed is being bought with quality.
6. Output: the definition verbatim, the headline rate with prior-period comparison if available, the breakdown table, the top three handoff patterns (which category bounces, and to whom, as patterns not blame), and a methodology note — period, scope, signals used for handoff detection, exclusions, searches run, caps hit.

## Guardrails

- The definition ships in the output, always, word for word — a bare percentage is not a deliverable.
- Never present capped searches as exact totals; disclose caps.
- Per-tech FCR is role-sensitive: escalation-tier engineers structurally cannot have high FCR because they receive the handoffs. Benchmark role-aware and never rank techs on this number alone.
- Aggregate, not enumerate — handoff patterns, not lists of bounced tickets.
- If handoff history is not reliably visible in the data, downgrade the deliverable honestly: report a bounded estimate ("between X% and Y% depending on treatment of Z") instead of a false-precision single number.
