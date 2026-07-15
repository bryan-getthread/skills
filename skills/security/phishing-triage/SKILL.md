---
name: Phishing Triage
description: Assess a reported suspicious email, contain it if malicious, and reply to the reporter with a clear verdict.
category: Security
tools: [search_tickets, search_knowledge_base, add_ticket_note, update_ticket]
---

# Phishing Triage

**When to use:** A user reported a suspicious email, or a phishing-report ticket landed on the security board.

**What you get:** A verdict with reasoning, containment actions where needed, and a reply to the reporter, logged as an incident.

## Steps

1. Capture sender, subject, time, links, and attachments. Do not click them.
2. Assess indicators: lookalike domain, urgency or payment lures, mismatched links, unexpected attachments.
3. Check whether it is a known phishing-simulation before treating it as a real threat.
4. Determine who else received it and whether anyone interacted.
5. If malicious, advise quarantine/removal and a password reset for anyone who clicked, then reply to the reporter with the verdict and log it.

## Guardrails

- Never open links or attachments from the reported email.
- Confirm simulation status before raising a real incident, and vice versa.

## Consolidates

Phishing report triage, phishing-simulation identification, and reported-email review skills.
