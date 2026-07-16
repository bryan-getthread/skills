---
name: Automation Failure Watch
description: Detect automation-error signatures in ticket notes (4xx/5xx response bodies, Jinja/template rendering errors, webhook failures), flag the ticket, and ping the right Teams channel so a broken flow gets noticed. Answers the commonly requested "tell us when our automations are silently failing" partner workflow.
category: Automation & Flows
tools: [search_tickets, add_ticket_note, "zapier: Microsoft Teams \"Send Channel Message\""]
---

# Automation Failure Watch

Broken automations fail quietly — an error lands in a note and the ticket looks normal. This skill recognizes the error signatures, flags the ticket, and posts to the team's Teams channel so someone fixes the flow instead of discovering it three weeks of failures later.

## When to use

- Flows or integrations occasionally dump error bodies into ticket notes and nobody watches for them.
- "Alert us when an automation errors out on a ticket."
- Catching template/rendering failures (unrendered `{{ }}` tokens, Jinja errors) before they ship to a client.

## Steps

1. Read the triggering note with `search_tickets`. Check it against the desk's configured automation-error signatures: HTTP 4xx/5xx status lines with response bodies, `Jinja`/template rendering errors, unrendered `{{ ... }}` or `{% ... %}` tokens, "webhook failed", stack-trace fragments, connector auth failures. **No signature match → stop.** This skill never fires on ordinary notes.
2. Post a plain-text internal note via `add_ticket_note` flagging the failure: which signature matched and the error excerpt (this doubles as the dedupe marker).
3. Resolve the correct Teams channel from the desk's documented automation-alert channel mapping (a broad flow failure and a single-client integration may go to different channels).
4. Send a compact message via `zapier: Microsoft Teams "Send Channel Message"`: ticket reference, client, the matched signature, and the error excerpt — enough for an engineer to triage without opening every ticket.

## Guardrails

- **Signature-gated.** Only configured error signatures fire; a note that merely mentions "error" in prose is not a match. If unsure whether a note is a real automation failure, it isn't.
- Dedupe: if a failure-flag note for this same error already exists on the ticket, do not ping again.
- **Member-connected connector:** the Teams Zapier action runs as the member who connected it; say so when configuring, and prefer a service member's connection for Flow use. Open with the verify-first gate — see `skills/connectors/zapier-action-discovery`.
- **Task-cost guardrail:** each Teams message consumes Zapier task budget; signature-gating protects it. Warn the desk if a signature is so broad it would fire constantly.
- **Degradation:** if the Teams Zapier action is unavailable (not connected, budget exhausted), the internal flag note is still posted so the failure is visible in-app; say the ping was skipped.
- Never quote credentials or tokens from an error body into the Teams message — redact them (see `skills/documentation/credential-storage-audit`).
- Notes plain text (PSA compatibility).

## Unattended (Flows) variant

- Fires on the **note-added event** (the error note landing) — a supported ticket event, not a timer.
- Runs as a Super Magic agent (a Flow cannot natively send a Teams message; it calls Run Skill / New Super Magic Agent and this skill uses the Zapier action).
- Your entire reply is the internal flag note, verbatim: `AUTOMATION FAILURE FLAGGED. Signature: <sig>. Teams: <channel | skipped-unavailable>.` or `NO ACTION.`
- Deterministic stops: no signature match → no action; already flagged for this error → no action; connector unavailable → flag note only, ping skipped.
