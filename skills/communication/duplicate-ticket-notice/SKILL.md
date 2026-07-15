---
name: Duplicate Ticket Notice
description: Tell a client their multiple tickets on the same issue have been consolidated — which ticket survives, where updates will land, and that nothing was lost.
category: Communication
tools: [search_tickets, view_openDraft, add_ticket_note]
---

# Duplicate Ticket Notice

Drafts the courtesy message after a merge: reassures the client that filing twice didn't lose anything, and points all future traffic at one thread.

## When to use

- "I merged their three tickets — let the client know."
- After a duplicate-detection merge, the client needs to know where their issue now lives.
- A client emailed a new ticket instead of replying, and their new ticket was folded into the original.

## Steps

1. Confirm the merge already happened and identify the surviving ticket (search_tickets). This skill communicates a completed consolidation — it does not decide or perform merges (that's the duplicate-detection workflow's job; if the merge hasn't happened, stop and say so).
2. Draft per the Email Baseline Standard skill (communication/email-baseline-standard), on the **surviving** ticket's thread:
   - We noticed multiple tickets about the same issue and combined them so nothing falls through the cracks — framed as service, never as a scolding for filing twice.
   - **Where updates will land:** "All updates will come through this ticket (<surviving ticket number>)."
   - Reassure: everything from the other ticket(s) was carried over; nothing was lost.
   - How to engage going forward: "just reply to this email" — one thread, one place.
3. Keep it to 3–4 sentences. If there's also a substantive status update, append it per communication/client-reply rather than sending two emails.
4. Offer a plain-text internal note via add_ticket_note on the surviving ticket recording which ticket numbers were consolidated and that the client was notified.
5. Present via view_openDraft.

## Guardrails

- Reference only ticket numbers that actually exist and were actually merged — verify, never recall from conversation memory.
- Tone check: the client did nothing wrong by opening duplicates. No "please avoid opening multiple tickets" unless the desk explicitly asks for that line.
- If the "duplicates" belong to different contacts at the client, name only the recipient's own tickets — never reveal what a colleague filed.
- This skill never merges, closes, or modifies tickets — notification only.
- Internal notes in plain text (PSA compatibility).
- Degradation: view_openDraft is in-app only — over external MCP, output the draft in chat labeled "DRAFT" with the surviving ticket number stated above it.
