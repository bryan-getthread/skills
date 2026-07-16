---
name: Recurring Issue Report
description: Someone asks which issues keep coming back — chronic problems hitting the same client or device repeatedly — and whether each has a root-cause fix underway.
category: Reporting & Analytics
tools: [search_tickets, search_clients, list_boards]
---

# Recurring Issue Report

Find the chronic issues — the same problem recurring on the same client, device, or user — and report each with its recurrence count, cost, and root-cause-analysis status, so repeat symptoms get fixes instead of another band-aid ticket.

## When to use

- "What issues keep coming back?" / "show me our chronic problems."
- "Has <client>'s server issue actually been root-caused or are we just closing tickets?"
- A monthly problem-management sweep feeding the RCA backlog.

## Steps

1. Confirm the period (default: trailing 30 days, with a 90-day lookback to catch slow repeaters) and pull resolved + open tickets with split searches per board per client segment; disclose caps.
2. Detect recurrence: **2 or more tickets in 30 days for the same client tied to the same device/asset or the same evident root cause**. Match on asset names, error signatures, and issue descriptions in thread text — categories alone under-match.
3. Verify each candidate cluster by reading the threads: same underlying problem, not coincidentally similar symptoms. Drop weak matches rather than padding the list.
4. For each confirmed chronic issue report: client, asset/user affected, recurrence count and dates, cumulative time spent (logged time where present, conservative estimate labeled as such otherwise), and **RCA status**:
   - **Fixed** — a root-cause fix shipped; recent recurrence predates it.
   - **RCA in progress** — a problem/project ticket exists and is active.
   - **Band-aid loop** — each occurrence closed symptomatically, no RCA anywhere.
5. Rank by cost × recurrence, lead with the band-aid loops, and recommend the next step per issue (open a problem ticket, schedule the replacement, escalate to vendor, client conversation).
6. Methodology note: period, matching criteria, searches, caps.

## Guardrails

- The 2-in-30-days threshold is the trigger for *investigation*, not proof of a chronic problem — thread verification is mandatory before an issue makes the report.
- Never mark RCA status without evidence in the tickets (a linked problem ticket, a fix note); absence of evidence = band-aid loop, and say the classification basis.
- Exclude junk/NOC noise recurrences (a flapping monitor is an alert-tuning finding, not a chronic issue — route it to the alert-noise assessment instead).
- Aggregate the report by issue; cite one representative ticket per issue, not the full recurrence list.
- Recommendations only — do not open problem tickets or merge recurrences without explicit confirmation.

## Running this unattended

> **Flows cannot schedule or time-trigger this.** Thread Flows fire on ticket *events* and conditions only — there is no schedule, cron, ticket-age, or elapsed-time trigger. This is a cadence/sweep skill, so run it **manually** on demand, or from an external scheduler that invokes Super Magic. A Flow can only reach it via **Run Skill** on a qualifying ticket event, never "every morning" or "after N hours". The output discipline below applies whenever it runs unattended.

- Follows the Unattended Output Discipline contract: the entire reply is the plain-text report — per confirmed chronic issue: client, asset/user affected, recurrence count and dates, cumulative time (labeled logged vs estimated), and RCA status — band-aid loops first, with the methodology line (period, matching criteria, caps) at the end. No narration.
- Deterministic inputs from the flow: the period and board scope. Thread verification stays mandatory: candidate clusters that cannot be verified as the same underlying problem are dropped, never padded in.
- No confirmed chronic issues → reply exactly `NO RECURRING ISSUES CONFIRMED.`
- Junk/NOC noise recurrences are excluded in every mode; capped searches make counts "at least N".
- Permitted writes: none. Opening problem tickets, merging, and client conversations stay attended.
