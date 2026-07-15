---
name: Outage Notification
description: Draft a major-incident or mass-outage client notice — known impact, what we're doing, when the next update comes — without speculating on cause.
category: Communication
tools: [search_tickets, view_openDraft]
---

# Outage Notification

Drafts the message clients get during an active outage: calm, factual, and specific about the one thing that keeps them from flooding the desk — when they'll hear from you next.

## When to use

- "Draft an outage notification for <service> being down."
- "We need to notify all affected clients about this incident."
- A major incident ticket needs its first client-facing notice or an interim update.

## Steps

1. Gather the confirmed facts from the incident ticket(s): what service/system is affected, since when, who/what is impacted, and what the team is actively doing. Confirmed means stated in the ticket record — not inferred.
2. Draft per the Email Baseline Standard skill (communication/email-baseline-standard), in this order:
   - **Known impact:** what's affected and what still works, in the client's terms ("email delivery is delayed" not "the SMTP relay is degraded").
   - **What we're doing:** the active response, one or two sentences ("our team is engaged and working with the vendor").
   - **Next update time:** a specific commitment — "we will send the next update by <time>, or sooner if the situation changes." This line is mandatory.
   - What the client should do meanwhile, if anything (usually: nothing, no need to open a ticket).
3. Keep it short. During an outage, clients read the first three lines.
4. For interim updates, lead with what changed since the last notice; for the all-clear, state restoration, confirm stability, and note that a post-incident summary will follow if one is planned.
5. Present as an open draft via view_openDraft. Mass distribution is a human action — provide the text, never the send.

## Guardrails

- **Never speculate on cause.** No "likely a cyber attack," no "appears to be <vendor>'s fault," no unconfirmed root cause of any kind. "We are investigating the cause" is the only pre-confirmation phrasing. Defensive-writing rules from the Email Baseline Standard apply in full.
- Never use "breach," "hacked," or "compromised" unless a confirmed security incident is formally declared — and then the security team's language governs.
- The next-update time is a real commitment: pick one the team will keep, and err generous.
- Never name other affected clients or reveal the blast radius across your client base.
- Degradation: view_openDraft is in-app only — over external MCP, output the notice in chat labeled "OUTAGE NOTICE DRAFT."
