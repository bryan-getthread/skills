---
name: Resolution Closing Email
description: Draft the closure email for a resolved ticket — what was wrong, what we did, how to reopen — built from the ticket's actual evidence, not memory.
category: Communication
tools: [search_tickets, view_openDraft]
---

# Resolution Closing Email

Drafts the final client-facing message when a ticket is resolved: a short, accurate account of the problem and the fix, with a clear path back if anything resurfaces.

## When to use

- "Draft the closing email for this ticket."
- "Write the resolution summary for the client."
- A ticket is moving to resolved/closed and the client deserves a proper close-out.

## Steps

1. Read the full ticket: original report, notes, time entries, and the resolving actions. The email is built only from what the record shows.
2. Draft per the Email Baseline Standard skill (communication/email-baseline-standard), covering three things in plain language:
   - **What was wrong** — the issue as the client experienced it (their words, not the internal classification).
   - **What we did** — the fix, summarized for a non-technical reader; translate jargon (see communication/technical-to-plain-english for heavy cases).
   - **How to reopen** — end with: "If anything isn't resolved, just reply to this email — replying reopens this ticket."
3. If the fix requires anything from the client going forward (a restart, a new password at next login), state it as an explicit next step.
4. Keep it to 1–2 short paragraphs plus the reopen line. A closure email is a receipt, not a report.
5. Present as an open draft via view_openDraft for review. Do not send, and do not change the ticket status — closure is the technician's call.

## Guardrails

- Only claim actions the ticket record actually documents. If the resolution notes are thin, draft what is supported and flag: "NEEDS: confirm what resolved this — notes don't say."
- Never state a root cause that wasn't established; "resolved after <action>" is honest, "caused by X" requires evidence.
- No credentials, internal notes, or other clients' details in the email.
- Degradation: view_openDraft is in-app only — over external MCP, output the draft in chat labeled "DRAFT — review before sending."

## Unattended (Flows) variant

- Your entire reply is the email body, verbatim — no subject-line labels, no narration.
- If the ticket record doesn't establish what was done to resolve the issue, output nothing — a wrong closure email is worse than none.
