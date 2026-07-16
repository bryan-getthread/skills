---
name: Atera Workflow
description: For desks synced to Atera — ticket lifecycle idioms (simple status set, resolved-vs-closed), contract types (retainer, block hours, monitoring, project), and the per-technician-pricing culture that shapes how Atera desks think about labor.
category: PSA-Specific
tools: [search_tickets, list_ticket_statuses, update_ticket, add_ticket_note, log_time_entry, search_clients]
---

# Atera Workflow

Applies to: **Atera**. Atera is an all-in-one PSA+RMM priced **per technician, not per endpoint** — which shapes desk culture: Atera shops tend to run lean tech counts, automate aggressively, and are less granular about labor cost per device. Tickets follow a deliberately simple lifecycle (Open → Pending → Resolved → Closed by default, tenant-customizable), and billing runs through **contracts** on each customer: Retainer (flat fee), Block Hours, Block Money, Hourly/T&M, Monitoring, Project, and Online Services. This skill keeps Thread-side actions consistent with those idioms.

## When to use

- Status changes or closure on an Atera-synced desk.
- Triage where "is this covered?" depends on the client's Atera contract type.
- Time logging on an Atera desk where the contract determines billability.
- Reconciling Thread↔Atera drift on a ticket.

## Steps

1. Re-fetch the ticket with `search_tickets` at full detail — Atera→Thread sync can lag, and Atera's automation rules (auto-assignment, auto-close of resolved tickets) change tickets without human action.
2. **Lifecycle:** pull the live status list with `list_ticket_statuses`; verify rather than assume the default four. Distinguish **Resolved** from **Closed**: many Atera desks use Resolved as a client-confirmation window with an automation that auto-closes after N days — jumping straight to Closed kills the reopen window. Pending-family statuses signal waiting-on-customer and often feed auto-follow-up rules; a Pending move requires a stated wait reason in the note.
3. **Contract read at triage:** determine the client's contract in effect — evidence order: contract fields visible on the synced ticket/client, the desk's documented client-contract sheet, comparable recent tickets. Classify the work: Retainer/Monitoring → covered (check the desk's exclusion list for projects); Block Hours / Block Money → covered while balance remains, and the balance usually is **not** visible from Thread — say so and recommend an Atera-side check before large efforts; Hourly/T&M → billable, set expectations first; Project → scope against the project, not the service contract. Never infer contract type from client size or tone; if nothing shows it, ask.
4. **Time entries:** log with `log_time_entry` against the correct ticket; note in plain text whether the session was contract-covered or billable per the classification above. On block-hour clients, follow the burn-down note convention ("1.5h this session; ticket total 4.0h") so the balance story stays reconstructible — without ever stating a contract balance you cannot see.
5. **Per-tech-pricing culture notes:** because Atera does not meter endpoints or hours in its own pricing, desks often lack per-device cost discipline — do not assume utilization or profitability data exists Atera-side. When asked profitability questions, route to `skills/finance-and-billing/agreement-profitability` with the caveat that labor data quality on Atera desks varies.
6. **RMM-side awareness:** alert-generated tickets carry device context; Thread has no Atera RMM tool surface, so device remediation is a handoff to a tech in Atera — recommend, never claim done.
7. **Drift:** when Thread and Atera disagree, follow `skills/psa-specific/psa-is-master-reconciliation` — fresh re-fetch first, then align Thread to Atera and record with a plain-text `add_ticket_note`.
8. Output: the action taken or proposed, contract classification with its evidence, side effects (auto-close windows, billability), and anything not visible from Thread stated as such.

## Guardrails

- **Sync lag:** re-fetch full ticket detail immediately before trusting or changing status; Atera automations mutate tickets continuously.
- **PSA is always master.** Thread moves to match Atera, never the reverse.
- Never set a status not returned by `list_ticket_statuses`; do not invent tenant-specific statuses or contract names.
- **Never guess coverage.** Low confidence → label "coverage unverified" and ask; a wrong covered/billable call is a revenue or trust incident.
- Never state or estimate a block-hour/block-money balance you cannot see; report "balance not visible from Thread."
- Respect the resolved→auto-close pattern; do not close early, and do not resolve over an unanswered client message.
- Notes syncing to Atera must be plain text — no markdown, no emojis. Never put rates or amounts in client-visible notes.

## Cross-references

- Contract-read siblings: `skills/psa-specific/autotask-contract-categories`, `skills/psa-specific/cw-agreement-aware-triage`
- Generic reconciliation: `skills/psa-specific/psa-is-master-reconciliation`
- Billing follow-through: `skills/finance-and-billing/billable-analysis`, `skills/finance-and-billing/out-of-scope-billing-flag`
