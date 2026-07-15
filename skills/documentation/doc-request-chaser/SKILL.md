---
name: Doc Request Chaser
description: Chase documentation the client owes the desk — network details, vendor contacts, sign-offs — with a polite escalating cadence and the three-attempts discipline, every attempt recorded on the ticket.
category: Documentation
tools: [search_tickets, add_ticket_note, view_openDraft]
---

# Doc Request Chaser

Runs the follow-up ladder for information only the client can provide: what's owed, why the work is waiting on it, and a three-attempt cadence that ends in a documented decision instead of a ticket that ages in silence.

## When to use

- "Still waiting on <client> for their ISP details / vendor contact / floor plan — chase it."
- Onboarding is blocked on client-supplied documentation and the request thread has gone quiet.
- An approval or sign-off the client owes is stalling a project task.

This is the documentation-flavored sibling of communication/followup-chaser (client replies on tickets) — use this one when the missing item is information or a document the desk needs *from* the client to proceed.

## Steps

1. Read the ticket (search_tickets) and establish the ledger before chasing: exactly **what was requested** (itemized — partial responses are common), **when**, **what has arrived**, and **what work is blocked** by each missing item. If the original request was vague, the first "chase" is actually a clean re-request — itemize it properly this time.
2. Count the prior documented attempts on the ticket. The cadence assumes attempts a reasonable interval apart (3–5 business days is the default; follow the desk's convention) — two chases in one afternoon do not count as two attempts.
3. Draft the next message per the Email Baseline Standard skill (communication/email-baseline-standard), matched to the attempt number:
   - **Attempt 1 (first chase):** friendly and assumption-free — the request may simply be buried. Restate the itemized list in checklist form (received ✓ / still needed), the easiest way to send each item, and *why* it's needed: "we can't schedule <work> until we have <item>."
   - **Attempt 2:** shorter, adds the consequence in neutral terms — the specific work that stays paused, and any date that slips. Offer the low-effort out: "happy to jump on a 10-minute call and take it down verbally."
   - **Attempt 3 (final chase):** plain statement that this is the last follow-up; the blocked work will be put on hold / the project re-planned around the gap on <date>, and one line on how to restart when they're ready. Blameless — per the spirit of communication/three-strikes-final-email, final means final, not passive-aggressive.
4. Recommend widening the audience at attempt 2–3 when the ticket shows a single unresponsive contact: CC the client's primary contact or account sponsor — a human/AM decision, suggested with reasoning, not done silently.
5. After each attempt is sent, record it via add_ticket_note (plain text per the Note Format Standard skill, documentation/note-format-standard): attempt number, date, channel, itemized list still outstanding. The evidence trail is what makes attempt 3 fair — and what protects the desk when the project delay is questioned later.
6. Present drafts via view_openDraft; sending is the technician's or AM's action.

## Guardrails

- **Three attempts, honestly counted:** documented, reasonably spaced, and itemized — an unanswerable vague request doesn't count as an attempt. Verify the evidence on the ticket before drafting attempt 3.
- Never fabricate consequences or deadlines: the "work paused on <date>" claim must be real and confirmed by the requester — an empty threat that isn't executed destroys the cadence's credibility.
- Tone floor: every rung stays polite and blame-free — the client is a customer who owes information, not a delinquent. No sarcasm, no "as per my last three emails."
- After the final attempt runs out, the outcome is a status decision (hold the ticket, re-scope, escalate to the AM) recorded on the ticket — not a fourth chase, and never silent closure of work the client still expects.
- Approvals with contractual or billing weight are chased in writing only — do not offer to "take it verbally" for those.
- Degradation: view_openDraft is in-app only — over external MCP, output the message in chat labeled "CHASE DRAFT — attempt <n> of 3."
