---
name: Shift Overflow Tracker
description: Someone asks how many tickets got reassigned away from a shift or tech, or wants assignment history reconstructed from notes to quantify overflow between shifts.
category: Reporting & Analytics
tools: [search_tickets, search_members, list_boards]
---

# Shift Overflow Tracker

Reconstruct assignment history from ticket notes and audit trails to quantify how much work flowed away from a shift or technician — who absorbed it, when, and whether the overflow is structural or situational.

## When to use

- "How many tickets got reassigned away from the night shift last month?"
- "Is work being pushed off <user>'s queue onto others?"
- Investigating whether a shift's coverage matches the work it actually keeps.

## Steps

1. Confirm the shift or tech in question, the period (default: last 30 days), and the shift's working hours. Pull candidate tickets with split searches per board (disclose caps): tickets created during the shift's hours, plus tickets that touched the shift's members.
2. **Reconstruct assignment history from evidence in each thread**: assignment-change notes, "reassigning to…" comments, handoff notes, and owner-at-close versus first responder. Thread notes are the source of truth here; where the trail is ambiguous, mark the ticket "unclear" rather than guessing.
3. Classify each reassignment away from the shift/tech:
   - **Legitimate handoff** — end of shift, scheduled follow-up in business hours, skill-based escalation.
   - **Overflow** — work the shift could and should have kept but pushed (capacity, avoidance, or triage-only behavior).
   - **Unclear** — evidence insufficient.
4. Quantify: total created during shift, kept-and-resolved, handed off legitimately, overflowed, unclear — plus who absorbed the overflow and the estimated hours displaced.
5. Read the pattern: concentrated on certain nights/ticket types (structural: coverage or skills gap) versus one bad week (situational). Present 1–2 hypotheses with the evidence, and what would confirm them.

## Guardrails

- Reconstruction from notes is inherently approximate — always report the "unclear" bucket size, and never present overflow counts as exact audit figures.
- Overflow is a process finding, not an accusation: role and shift context matter (an overnight triage-only model *by design* hands everything off — confirm the intended model before labeling anything overflow).
- Manager's eyes only; do not post findings to tickets or channels the affected techs share, and keep individual names out of anything wider than the requesting manager.
- Never treat handoff volume alone as a performance signal; pair with the shift's intended role and its kept-work quality.
- Do not fabricate assignment events; every counted reassignment must cite thread evidence.
