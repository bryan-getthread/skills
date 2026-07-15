---
name: Security Alert Response
description: Triage a SOC, breached-credential, or dark-web alert: assess exposure, contain, and route to the right client and workflow.
category: Security
tools: [search_tickets, search_clients, add_ticket_note, update_ticket, search_itglue]
---

# Security Alert Response

**When to use:** A security alert ticket arrives (breached credentials, dark web, SOC detection, user-created alert) and needs structured handling.

**What you get:** A triaged alert with exposure assessed, the correct client attached, containment or closure actions taken, and an internal record.

## Steps

1. Parse the alert: affected user, client, indicator, and the exposed value or detection detail.
2. Attach it to the correct client if it arrived on a shared alert company.
3. Assess exposure and recency. For example, decode a leaked value and judge whether it is current or already rotated.
4. Decide severity and the response: contain and escalate a live threat, or auto-close a stale or benign alert with a note.
5. Record the decision and reasoning as an internal note and set the appropriate status.

## Guardrails

- Do not auto-close a live or recent exposure. Age and validity decide the path.
- Follow the client's incident policy for anything that looks like an active compromise.

## Consolidates

Breached-credential response, dark-web-alert auto-close, security incident investigation, SOC user-created-alert, and SecOps triage/classification skills.
