---
name: Reminder Site Contact Escalation
description: On the final no-response reminder, also notify the site contact by email before the ticket closes — a last escalation up the client's own chain when the primary contact has gone dark. Extends the three-strikes final email. Answers the commonly requested "loop in the site manager before we close an unanswered ticket" partner workflow.
category: Escalation
tools: [search_tickets, search_contacts, add_ticket_note, "zapier: Microsoft Outlook \"Send Email\""]
---

# Reminder Site Contact Escalation

When the primary contact has ignored every reminder, closing silently can burn the relationship — the site manager may not even know their user went dark. This skill fires as the final reminder goes out: it verifies the attempts are real, then emails the site contact as a last escalation before closure.

## When to use

- The reminder ladder has reached its final "we're about to close this" step with no client response.
- "Before we close an unanswered ticket, tell the site manager."
- Escalating up the client's own chain when the requester is unreachable.

## Steps

1. Read the ticket with `search_tickets` and verify the evidence: the documented outreach attempts with no substantive response, and that this is genuinely the **final** reminder step (see `skills/communication/three-strikes-final-email` for the three-attempt bar). If the attempts aren't documented, stop.
2. Resolve the **site contact** for the ticket's site via `search_contacts` — the escalation contact for that location, distinct from the unresponsive primary contact. If no site contact resolves, stop and note it.
3. Compose a brief, neutral email to the site contact: what the ticket was for, that the primary contact hasn't responded to repeated attempts (with dates), and that the ticket will close unless someone follows up — no blame, no drama.
4. Send via `zapier: Microsoft Outlook "Send Email"` to the site contact.
5. Post a plain-text internal note via `add_ticket_note`: that the site contact was escalated to before closure, with the attempt dates.

## Guardrails

- **Evidence gate:** no escalation email without the documented final-reminder attempts on the ticket. Undocumented "I'm sure we tried" does not count.
- Site contact comes from `search_contacts` and the documented site-escalation mapping only — never a fabricated address, never the primary contact (who is the one who went dark).
- This skill does not close the ticket — closure remains a human/flow action. It adds one escalation step before that.
- Tone is neutral and blameless; the primary contact may have had a genuinely bad month.
- **Member-connected connector:** the Outlook Zapier action sends from the connecting member's mailbox — prefer a service member for Flow use; open with the verify-first gate (`skills/connectors/zapier-action-discovery`).
- **Task-cost + degradation:** each send spends Zapier budget; if unavailable, post an internal note directing a human to escalate to the site contact manually, and say the email was skipped.
- Notes plain text (PSA compatibility).

## Unattended (Flows) variant

- Fires on the **event of entering the final-notice status** (the reminder ladder's last step) — a supported status-change event, not "N days after" anything.
- Runs as a Super Magic agent (a Flow cannot natively send email; it calls Run Skill / New Super Magic Agent).
- Verify the documented attempts inside the run; if they aren't all present, output nothing.
- Your entire reply is the internal note, verbatim: `SITE CONTACT ESCALATED before closure: <contact>. Attempts: <dates>. Email: <sent | skipped-unavailable>.` or `NO ACTION. Reason: <attempts not documented | no site contact>.`
