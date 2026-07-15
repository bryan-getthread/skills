---
name: Three Strikes Final Email
description: Draft the final "we're closing this ticket" email after three documented contact attempts with no client response — only when the evidence of prior attempts actually exists.
category: Communication
tools: [search_tickets, view_openDraft, add_ticket_note]
---

# Three Strikes Final Email

The last message on an unresponsive ticket: professional, final, and blameless — sent only when the ticket record proves the desk earned it.

## When to use

- "Three attempts, no response — draft the final email."
- "Send the we're-closing-this notice on this ticket."
- The stale-ticket process (see communication/followup-chaser) has exhausted its ladder.

## Steps

1. Read the ticket thread and verify the evidence: **three documented outreach attempts** (emails, calls with notes, or voicemails recorded on the ticket) with no substantive client response since. List the three attempts with dates before drafting.
2. If fewer than three attempts are documented, do not draft the final email. Say which attempts are on record and route to communication/followup-chaser for the next rung instead.
3. Draft per the Email Baseline Standard skill (communication/email-baseline-standard):
   - One sentence of context: what the ticket was for.
   - The factual record: "We reached out on <date 1>, <date 2>, and <date 3> and haven't heard back."
   - The action: we're closing the ticket now, stated plainly and without reproach.
   - The open door, verbatim close: "If you still need help with this, just reply to this email — replying reopens this ticket."
4. Tone check: neutral and warm. No guilt-tripping ("despite our repeated attempts..."), no passive aggression. The client may have had a genuinely bad month.
5. Present as an open draft via view_openDraft. Offer a plain-text internal note via add_ticket_note recording the three attempt dates and that the final notice was sent. Closing the ticket itself is the technician's action, not yours.

## Guardrails

- Hard gate: no draft without three documented attempts in the ticket record. Undocumented "I'm sure we called them" does not count.
- Never close the ticket yourself; draft the email, note the evidence, and leave status changes to a human unless a flow explicitly owns closure.
- Dates in the email must match the ticket record exactly — this message is the audit trail.
- Internal notes in plain text only (PSA compatibility).
- Degradation: view_openDraft is in-app only — over external MCP, output the draft in chat labeled "FINAL NOTICE DRAFT" with the three attempt dates listed above it for verification.

## Unattended (Flows) variant

- Verify the three documented attempts from the ticket record inside the run. If you cannot find all three, output nothing — do not approximate.
- Your entire reply is the final email body, verbatim — no narration.
