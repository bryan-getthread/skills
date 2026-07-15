---
name: Time Entry Compliance
description: A manager asks who worked tickets without logging time, how much unbilled work is slipping through, or for a time-entry compliance sweep.
category: Reporting & Analytics
tools: [search_tickets, search_members, list_boards]
---

# Time Entry Compliance

Find tickets where engineers demonstrably did work but logged no time, quantify the unbilled-work exposure, and give each engineer a specific fix-it list — because unlogged time is silent margin leak.

## When to use

- "Who worked tickets this week without logging time?"
- "How much unbilled work are we leaking?"
- A recurring end-of-week time-hygiene sweep before invoicing.

## Steps

1. Confirm the period (default: the current week, aligned to the billing cycle if stated) and the boards in scope; exclude junk/NOC boards where no-time closes are expected.
2. Run split searches per engineer per board for tickets with activity in the period (notes, status changes, closures); disclose caps.
3. For each ticket, check for time entries covering that activity. Flag tickets with **work evidence but no time**: substantive notes, a closure, or a client reply authored by the tech, with zero minutes logged.
4. Distinguish severity:
   - **Likely billable, unlogged** — real troubleshooting or closure work on a billable client/agreement.
   - **Trivial touch** — one-line note or status nudge; flag softly, don't inflate exposure with these.
5. Estimate exposure: count of affected tickets per engineer and a conservative hours range (state the per-touch assumption used). Never present the estimate as an exact dollar figure unless rates were provided.
6. Output per engineer: their fix-it list (ticket + date + what the evidence shows), so time can be back-filled while memory is fresh. Aggregate the leadership view: totals and trend, not a shame list.

## Guardrails

- Absence of a time entry is evidence of a *logging* gap, not proof of unbilled work — some touches are legitimately non-billable. Present as "needs review," never as an accusation.
- Never log or back-fill time entries yourself — the engineer who did the work must attest to the duration. Zero-assumption rule: a recommendation is not an action.
- Role-aware: dispatchers and coordinators touch many tickets administratively; exclude pure routing touches from their counts.
- Manager and engineer eyes only; keep names out of any wider report — aggregate exposure only.
- Disclose result caps; a capped sweep understates exposure and should say so.
