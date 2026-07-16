---
name: Status Query Autoresponder
description: When a reply on an idle or closed thread is just asking "what's the status?", reply with the current status and the last client-visible update — and do nothing if the reply is anything more than a status check. Answers the commonly requested "auto-answer where's-my-ticket questions" partner workflow.
category: Triage & Routing
tools: [search_tickets, view_openDraft, add_ticket_note]
---

# Status Query Autoresponder

"Any update on this?" replies clog the queue and usually just need the current status echoed back. This skill answers a genuine status-check reply with the ticket's current status and its last client-visible update — and stays silent on anything that is actually a new request, question, or complaint.

## When to use

- Idle or closed threads get "what's happening with this?" replies that just need a status.
- "Auto-answer status-check messages so techs don't have to."
- Reducing "where's my ticket" pings without auto-replying to real requests.

## Steps

1. Read the reply and the ticket with `search_tickets`: current status, and the most recent **client-visible** update on the thread.
2. Confirm the reply is a **pure status check** — asking for progress/status with no new request, no new symptom, no complaint, no attachment for action. Evaluate the whole message in its language. Any additional content → do nothing.
3. Compose a short, client-appropriate reply: the current status in plain terms and a one-line paraphrase of the last client-visible update (and a next step if one is genuinely on record). Do not invent progress that isn't in the ticket.
4. Present it as a draft via `view_openDraft` for the tech to send. Post a plain-text internal note via `add_ticket_note` recording that a status response was drafted/sent.

## Guardrails

- **Status checks only.** If the reply contains a new question, request, symptom, or complaint, do nothing — those need a human.
- **Never fabricate status.** Report only the actual current status and the update already on the ticket. If there is no meaningful update, say the ticket is in progress and a tech will follow up — do not manufacture detail.
- Do not reopen or change status; a status reply on a closed ticket does not mean it should reopen (see `skills/triage-and-routing/re-fw-reopen-detection`).
- Degradation: `view_openDraft` is in-app only; over external MCP, output the reply in chat labeled "STATUS REPLY DRAFT" and let the tech send it.
- Internal notes plain text (PSA compatibility).

## Unattended (Flows) variant

- Fires on the **reply event** — a supported ticket condition.
- Runs as a Super Magic agent; the client-facing send is the flow's **Reply** action (or the skill's draft in attended mode). Your entire reply is the client message body, verbatim — no narration.
- Deterministic stops: reply is not a pure status check → do nothing; no real status to report → do nothing (let a human handle it).
- Never reopen the ticket; never change status. When in doubt, do nothing.
