---
name: Report Phishing Guide
description: Draft reply-ready instructions telling an end user what to do with a suspicious email — the client's report-button path, never forwarding it around — "tell the user how to report a phish."
category: End-User Guides
tools: [search_tickets, search_knowledge_base, search_itglue, search_hudu, view_openDraft]
---

# Report Phishing Guide

Produces a client-ready instruction block for the user staring at a suspicious email: report it through the client's actual reporting mechanism, don't click anything, don't forward it to colleagues "to warn them." Also the standing guide a desk sends proactively after a phishing wave.

## When to use

- "User asked what to do with a suspicious email — send them the reporting steps."
- After a phishing verdict (see security/phishing-triage): the "here's what to do next time" education reply.
- Proactive broadcast material during a phishing campaign against a client.

## Steps

1. **Identify the client's reporting mechanism first** (search_itglue / search_hudu / search_knowledge_base / search_tickets): the built-in Outlook Report button, a security-vendor add-in button (KnowBe4 Phish Alert, Huntress, Proofpoint, etc. — name whichever the docs show), or a documented report-to mailbox. Also check whether the button exists on both desktop and mobile for this client. If no mechanism is documented, ask the technician ONE question — telling a user to press a button they don't have teaches them to ignore you.
2. **Check the urgency branch.** If the ticket suggests the user already clicked a link or entered credentials, this is not an education reply — route to security/phishing-triage and the tech immediately; the user-facing message then comes from that incident flow, not this guide.
3. **Draft the instruction block** to end-user rules, one action per step:
   - Open with the calm frame: "You did the right thing by asking. Here's the safe routine, every time."
   - The don'ts, plainly and first: don't click links, don't open attachments, don't reply to it, and don't forward it to coworkers — forwarding spreads the dangerous link and breaks the technical trail we use to trace it.
   - The report path with a what-you'll-see cue ("in the toolbar at the top of the message, look for the button named <documented button> — after you press it the email disappears from your inbox; that's normal and means it worked").
   - What happens next, honestly: who sees the report and roughly when they'll hear back (per client docs), or "you won't hear back unless action is needed" if that's the documented behavior.
   - The it-might-be-real note: "If it turns out to be a legitimate email, no harm done — we'd always rather you report ten real ones than miss one fake."
   - Off-ramp: "If you already clicked a link or typed your password anywhere, call the desk right now instead of emailing — that changes what we do and minutes matter."
4. **Assemble per the Email Baseline Standard** (communication/email-baseline-standard) — including its defensive-writing rules (no "you were hacked" language) — and present via view_openDraft. Do not send.

## Guardrails

- **The never-forward rule is the point of this skill** — it appears in every draft, with the plain reason. The one documented exception: if the client's mechanism IS a report mailbox, describe the forward-as-attachment routine exactly as their docs specify, and still ban forwarding to anyone else.
- Mechanism match is mandatory; do not describe a vendor button the client doesn't run.
- This guide never renders a verdict on a specific email — verdicts belong to security/phishing-triage. If the user attached the suspect email, hand off there first.
- No admin steps (quarantine review, mail-flow rules, purge actions) in the user block.
- Never include the suspicious link or a defanged copy in the outgoing draft.
- Localizable; the clicked-already off-ramp must survive translation with its urgency intact.
- Degradation: view_openDraft is in-app only — over external MCP, output the draft labeled "DRAFT — review before sending." Docs tools only when enabled.

## Cross-references

- security/phishing-triage — the verdict and incident flow this guide is the education tail of.
- communication/security-advisory-broadcast — the one-to-many version during a campaign.
- communication/email-baseline-standard.
