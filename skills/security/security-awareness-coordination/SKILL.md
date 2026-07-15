---
name: Security Awareness Coordination
description: A security-awareness training rollout needs coordinating — enrollment tracking, respectful nudges for people who haven't finished, and a completion report the client can hand to their insurer or auditor.
category: Security
tools: [search_tickets, search_contacts, search_clients, add_ticket_note, update_ticket, create_ticket, view_openDraft]
---

# Security Awareness Coordination

Runs the unglamorous middle of a training program: who's enrolled, who's finished, who needs a nudge (and how many they've had), and a clean completion report at the end. The training platform teaches; this skill makes sure everyone actually gets through it.

## When to use

- A client's awareness-training campaign kicked off and the desk owns enrollment/completion tracking
- "Who still hasn't finished the security training?" or a nudge round is due
- The campaign window is closing and the client needs a completion report for compliance or insurance

## Steps

1. Establish the roster: pull the in-scope user list from the campaign ticket or the client's records (search_contacts for the client's active users). Reconcile against reality — new hires since launch need enrolling, departed users need removing from the denominator, and exclusions (service accounts, shared mailboxes) get documented so the completion percentage is honest.
2. Track completion against the platform's export or the client program owner's report (the agent does not have direct training-platform access unless a connector provides it — work from the provided export and record its as-of date). Maintain the status split: completed / in progress / not started, in a plain-text note on the tracking ticket.
3. Laggard nudges — respectful, escalating gently:
   - First nudge: friendly direct reminder with the deadline and the time commitment ("about 20 minutes"), drafted for human send (view_openDraft where available). Assume good faith — people are busy, not negligent.
   - Second nudge: same tone, copy nobody, mention the compliance driver if there is one.
   - Still outstanding near deadline: hand the remaining list to the client's designated program owner (usually a manager or HR) — internal follow-up is their lever, not the desk's. Never shame, never mass-CC, never threaten access consequences unless the client's own written policy states them.
4. Handle friction as signal: users reporting broken links, locked accounts, or inaccessible modules get a support ticket (create_ticket), not another reminder — a nudge for a training they cannot open erodes trust in the whole program.
5. Completion reporting to the client: completion rate against the reconciled roster, the exclusions and why, remaining names delivered only to the program owner, and the as-of date of the underlying data. If the number comes from a capped or partial export, say so. Compliance-driven campaigns: note the evidence the client should retain (platform certificates/exports), and cross-reference the compliance-questionnaire-assist or cyber-insurance-form-prep skill when the report feeds one of those.
6. Recommend the next cycle's fixes: chronic-laggard patterns, enrollment gaps for new hires (propose adding training to the new-hire-onboarding checklist), and whether the cadence is working.

## Guardrails

- Nudges stay respectful and individual — no public lists, no mass-CC shaming, no invented consequences.
- The completion percentage is only as honest as the roster: reconcile hires/leavers/exclusions before reporting, and state the data's as-of date.
- Never fabricate completion status; work only from the platform export or program-owner confirmation, and never mark anyone complete on their own say-so when the platform disagrees — flag the mismatch instead.
- Named non-completers go to the program owner only; cohort numbers go in general reporting.
- Enforcement (access consequences, HR steps) belongs to the client, per their written policy — the desk coordinates, it does not police.
- Degradation: no training-platform connector means tracking runs on provided exports; state the as-of date rather than implying live data.
