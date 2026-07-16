---
name: Autotask Contracts Blocks
description: For desks synced to Autotask — deeper contract mechanics beyond type labels: block-hour burn tracking from visible time entries, retainer-threshold alerting conventions, and the edge cases (exclusions, overlapping contracts, expired-contract work) that misclassify coverage.
category: PSA-Specific
tools: [search_tickets, search_clients, update_ticket, add_ticket_note, log_time_entry]
---

# Autotask Contracts Blocks

Applies to: **Autotask (Datto/Kaseya)**. Extends `skills/psa-specific/autotask-contract-categories` (the triage-time coverage read) into the mechanics underneath: how block-hour and retainer balances actually burn, what the desk's alert thresholds mean, and the contract edge cases where the simple type label gives the wrong answer. Use the base skill to label a ticket; use this one when the question is about the *balance*, the *burn*, or a coverage call that the type label alone can't settle.

## When to use

- "How many hours has this client burned this month?" or "are they close to exhausting their block?" on an Autotask desk.
- A retainer/block client's usage is trending toward exhaustion and someone wants the early-warning note or escalation.
- Coverage is contested: work spans an exclusion, two contracts could apply, or the contract may have lapsed.
- Reviewing whether burn-down notes on hour-based clients are being kept per convention.

## Steps

1. Re-fetch the relevant ticket(s) with `search_tickets` and confirm the client with `search_clients`. Coverage and burn read against the wrong company is worse than no read.
2. **Establish what balance data you actually have.** The contract's true remaining balance lives in Autotask and is usually not synced into Thread. What Thread can evidence: time entries on tickets you can see, and burn-down notes left per the desk's convention ("2.0h this session; ticket total 5.5h"). Sum only what is visible, present it as "visible burn: at least Xh across N tickets in <window>," and if any search may have hit a result cap, say the figure is a floor, not a total. Never present visible burn as the contract balance.
3. **Burn-rate read (hour-based contracts).** From visible entries, characterize the trend: hours per week, notably heavy tickets, and whether the pace would exhaust a block of the size the desk documents — phrased conditionally ("at ~4h/week, a 20h block lasts ~5 weeks") without asserting the actual remaining balance. Recommend an Autotask-side balance check before anyone acts on the projection.
4. **Retainer alert conventions.** If the desk documents thresholds (commonly 75%/90% consumed, or Autotask's own contract notifications), your job is the Thread-side half: when visible burn plausibly crosses a threshold, leave a plain-text `add_ticket_note` flagging it and route the client conversation per the desk's playbook (account manager, not the tech). Do not message the client about money directly.
5. **Edge cases — resolve by evidence, else escalate:**
   - **Exclusions:** recurring contracts commonly exclude projects, after-hours, or named services. If the work matches a documented exclusion, it's billable even on a "covered" client — label per `skills/finance-and-billing/out-of-scope-billing-flag`.
   - **Overlapping contracts:** a client can hold a recurring contract *and* a block for projects. Which one the ticket burns is an Autotask-side setting on the ticket; if not visible, say "contract attribution not visible from Thread" and ask before labeling.
   - **Expired/lapsed contract:** work performed after end-date is usually T&M by default — but renewal may be in flight. Flag it; never assume either way.
   - **Zero-dollar/internal contracts:** some desks carry internal or warranty contracts; work on them is covered-but-unbillable. Follow the desk's sheet.
6. **Time entries you create:** `log_time_entry` per the desk's convention, always paired with the burn-down note on hour-based clients. Never adjust or delete existing entries — corrections are Autotask-side human work (`skills/finance-and-billing/time-entry-revenue-audit` for sweeps).
7. Output: what is visible vs not visible, the burn evidence with its floor-honesty caveat, the coverage determination or the explicit escalation, and any notes left.

## Guardrails

- **Sync lag:** re-fetch full detail before any read or write; contract association on a ticket can be corrected Autotask-side at any time.
- **PSA is always master.** Contract terms, balances, and attribution live in Autotask; Thread-side figures are evidence, never authority.
- **Never state or estimate a contract's remaining balance** unless it is actually visible from Thread. "Visible burn ≥ Xh; balance not visible" is the required phrasing.
- Never guess coverage on edge cases — exclusions, overlaps, and lapses go to a human when evidence is incomplete. A wrong covered/billable call is a revenue or trust incident.
- Threshold flags go to the desk's designated owner (account manager); never raise billing anxiety directly with the client.
- Plain-text notes only; never put rates, amounts, or balance speculation in notes the client can see.
- Degradation: without contract data synced into Thread, run advisory-only from the desk's documented sheet and visible time entries — state that limitation in every output.

## Cross-references

- Base coverage read: `skills/psa-specific/autotask-contract-categories`
- Billability follow-through: `skills/finance-and-billing/billable-analysis`, `skills/finance-and-billing/out-of-scope-billing-flag`
- Entry-level auditing: `skills/finance-and-billing/time-entry-revenue-audit`
