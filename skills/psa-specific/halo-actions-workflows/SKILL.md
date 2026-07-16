---
name: Halo Actions & Workflows
description: For HaloPSA desks — the deeper action/workflow layer beyond a single status change: approval actions, multi-step workflows, and knowing which action is valid at the ticket's current workflow step.
category: PSA-Specific
tools: [search_tickets, list_ticket_statuses, update_ticket, add_ticket_note, send_approval]
---

# Halo Actions & Workflows

Applies to: **HaloPSA**. Extends `skills/psa-specific/halo-status-actions`. That skill maps a single intent (respond / hold / resolve / close) to the Halo Action that carries the right status, note visibility, and notifications. This one goes a layer deeper: Halo tickets also move through **configured workflows** — ordered steps, each of which exposes a *different set of valid actions* — and some actions are **approval actions** that hand the ticket to an approver and block progression until a decision comes back. Acting without reading the current step is how you offer an action Halo won't allow, or skip an approval the process requires.

## When to use

- A Halo ticket is mid-workflow (change process, procurement, onboarding) and someone asks "what's the next step" or "can I move this forward".
- An action you expect (Resolve, Escalate) isn't valid right now and you need to know why.
- A ticket is waiting on an approval, or someone asks you to send one, on a Halo-synced desk.
- Mapping a desk's Halo workflows/approval steps to what is safe to do from Thread.

## Steps

1. Re-fetch the ticket at full detail with `search_tickets` before anything — Halo→Thread sync lags, and workflow position is exactly the field most likely to be stale (see `skills/psa-specific/halo-sync-audit`).
2. Establish where the ticket sits in its workflow: which workflow it's on and which **step/stage** it currently occupies. The set of legal next actions is a property of the *step*, not of the ticket globally — a Resolve action valid at "Work in progress" may not exist at "Awaiting approval".
3. Identify whether the current step is an **approval gate**. If the ticket is awaiting a decision, the only forward moves are approve / reject by the designated approver — a status nudge from Thread does not satisfy or bypass the gate. Report who the approver is if visible; do not self-approve on their behalf.
4. When the intent is to request an approval, use `send_approval` — and first confirm the desk actually routes approvals through Thread rather than through Halo's own approval action. If Halo owns the approval process, the request must originate in Halo; say so instead of firing a parallel Thread approval that the workflow won't recognize.
5. For a normal (non-approval) forward move, translate the target step's action into its full effect set exactly as `halo-status-actions` prescribes — status (verify via `list_ticket_statuses`), note visibility, notification, SLA effect — and apply status + note together in one pass.
6. Output: current workflow + step, the valid actions at this step, whether an approval is pending (and on whom), the proposed action with its effects, and the exact write. When the workflow position is ambiguous or the approval owner is unclear, propose and stop.

## Guardrails

- **Sync lag first.** Never judge workflow position or approval state from a list view or earlier turn — re-fetch full detail immediately before acting.
- **Never skip or fake an approval gate.** A status change from Thread does not equal an approval decision. Do not move a ticket past an approval step by editing status, and never approve on the approver's behalf.
- **Don't offer an action the current step doesn't expose.** Legal actions are per-step; if the desired action isn't valid here, say what step would make it valid rather than forcing a raw status edit.
- Respect the action model from `halo-status-actions`: status never travels without the note visibility and notification the action implies.
- Confirm where approvals live (Thread `send_approval` vs Halo's native approval action) before sending — a duplicate approval path confuses the workflow and the approver.
- Notes that sync to Halo are plain text — no markdown, no emojis.
- When in doubt — ambiguous step, unclear approver, uncertain approval ownership — do nothing and report.

## Cross-references

- Single-action mechanics: `skills/psa-specific/halo-status-actions`
- Note visibility per action: `skills/psa-specific/psa-note-visibility-rules`
- Status/workflow divergence sweep: `skills/psa-specific/halo-sync-audit`
- SLA effects of actions: `skills/psa-specific/halo-sla-and-priorities`
