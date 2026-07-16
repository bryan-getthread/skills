---
name: Status Nudger
description: Chase tickets stuck in a waiting status — re-send the pending approval or post the templated client nudge for that status, never nudging twice within the window. Run as a manual or scheduled sweep: the "after N hours in status" dwell trigger is NOT automatable by Flows today (Flows have no duration/age condition); a Flow can only post an immediate nudge the moment a ticket enters the status.
category: QA & Closure
tools: [search_tickets, send_approval, add_ticket_note, list_ticket_statuses]
---

# Status Nudger

Per-status chasing: when a ticket has sat in a waiting status past its configured dwell time, fire the right nudge for that status — re-send the approval for "Waiting on Approval", post the templated client follow-up for "Waiting on Customer". Reading dwell (time since the ticket entered the status) from the ticket at run time is fine; what Flows cannot do is *trigger* on that elapsed time. So run this as a manual or scheduled sweep. A Flow can attach to the status-change event to post an *immediate* on-entry message, but not a delayed "after N hours" nudge. Sibling of `qa-and-closure/stale-ticket-followup-cadence` (the general queue sweep); this one is the single-status version.

## When to use

- A Flow fires on a ticket entering "Waiting on Approval" / "Waiting on Customer" / "Waiting on Vendor" and the desk wants an automatic follow-up after N hours in that status.
- The desk wants different nudge wording and timing per status, not one generic reminder.
- Approvals go unanswered and nobody remembers to re-send them.

## Steps

1. Confirm the ticket is still in the triggering waiting status. If it has moved on, stop — the wait resolved itself.
2. Compute dwell: time since the ticket entered this status. If dwell < the configured window for this status (defaults: Waiting on Approval 48h, Waiting on Customer 72h, Waiting on Vendor 96h — the desk's config in the skill overrides these), do nothing.
3. Check for a prior nudge within the current window: scan the ticket's recent notes/messages for this skill's nudge marker ("[status-nudge]" in the internal note). If one exists inside the window, do nothing — never nudge twice within the window.
4. Fire the status-appropriate action:
   - **Waiting on Approval** → re-send via `send_approval` to the same approver with the original summary. Do not invent a new approver.
   - **Waiting on Customer** → post the desk's per-status client template (encoded in this skill's config section) as the outgoing message: brief, friendly, restates what is needed from <client contact>, no new commitments.
   - **Waiting on Vendor / other waits** → internal note reminding <owner> to chase, with days waited stated.
5. Record an internal note via `add_ticket_note` containing the "[status-nudge]" marker, the status, the dwell time, and what was sent — plain text, PSA-safe.

## Guardrails

- One nudge per window, ever. The marker check in step 3 is the non-negotiable gate; if you cannot read the ticket history to verify, do not nudge.
- Templates only — never freestyle a client-facing message unattended. If no template is configured for this status, post the internal reminder note instead and say a template is missing.
- Never change the ticket's status, owner, or priority; this skill only re-sends and reminds.
- Respect stop signals: if the latest message on the ticket is from the client or approver (they already replied), do nothing — a nudge after a reply reads as ignoring them.
- Do not invent approver identities, links, or deadlines. Re-send only what was originally sent.

## Running this unattended

> **Flows cannot fire on dwell time.** Thread Flows have no duration/age trigger, so the "after N hours in status" behavior runs manually or from an external scheduler, not from a Flow. The one Flow-native slice is the *immediate* on-entry message (attach a Reply/Note action, or Run Skill, to the status-change event). The discipline below applies whenever it runs unattended.

- Your entire reply is posted verbatim as the internal note — plain text, no narration, no markdown, no questions.
- Deterministic stops, in order: status changed → do nothing; dwell under window → do nothing; prior nudge in window → do nothing; client/approver already replied → do nothing; no template configured for a client-facing nudge → internal note only.
- When any check is ambiguous (cannot determine dwell, cannot read history), do nothing. A missed nudge costs a day; a duplicate or misplaced nudge costs trust.
