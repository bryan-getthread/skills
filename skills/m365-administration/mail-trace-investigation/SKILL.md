---
name: Mail Trace Investigation
description: Run a disciplined Exchange Online message trace — tight timeframe, sender/recipient pair, verdict reading, and the extended (historical) trace path for anything older than 10 days. Use when a ticket needs proof of what happened to a specific message or mail between two parties.
category: M365 Administration
tools: [search_tickets, search_contacts, add_ticket_note, log_time_entry, web_search]
---

# Mail Trace Investigation

Turns "the email never arrived" into a verdict with evidence: where the message
went, what the filter decided, and what to do about it. The agent frames the
trace parameters and reads the results the tech pastes back; the tech runs the
trace in the Exchange admin center or Defender portal.

## When to use

- "Did <sender>'s email to <recipient> ever arrive?"
- "Prove we sent it" / "prove they sent it" disputes.
- Feeding evidence into a delivery diagnosis (mail-flow-delivery owns the
  broader NDR/bounce playbook; this skill owns the trace mechanics).
- Checking whether a batch of messages was quarantined, dropped, or delivered.

## Steps

1. Extract the trace parameters from the ticket before anyone opens a portal:
   sender address, recipient address, and the narrowest defensible time window
   (the reported send time ± a few hours — never "last 30 days" as a first
   pass; wide traces bury the answer). Get message subject or Message-ID if
   the requester has it.
2. Pick the right trace type by age:
   - ≤ 10 days: standard message trace (near-real-time). PowerShell equivalent
     `Get-MessageTrace -SenderAddress <s> -RecipientAddress <r> -StartDate <t1>
     -EndDate <t2>` — verify against current module versions.
   - > 10 days: extended/historical search (`Start-HistoricalSearch`), which
     runs asynchronously and returns a CSV — set the expectation in the ticket
     that results can take hours, and that history is capped (roughly 90 days);
     older than that, there is no trace to run.
3. Have the tech run it and paste the results. Read the status/verdict
   honestly:
   - Delivered — landed in the mailbox; the problem is client-side (rules,
     Focused Inbox, moved/deleted). Point at the folder via
     `Get-MessageTraceDetail` if needed.
   - FilteredAsSpam / Quarantined — filter verdict; route to
     quarantine-release-request for the release decision, and
     anti-spam-policy-tuning only if a pattern emerges.
   - Failed — read the detail event and the NDR code; hand to
     mail-flow-delivery for the bounce diagnosis.
   - Pending / Getting status — still in transit or throttled; re-trace after
     a defined interval rather than guessing.
   - No results — the message never reached Exchange Online: wrong address,
     sender-side failure, or it went to a different mail system. Say that
     plainly; absence of a trace row is itself evidence.
4. If the question is "who else got this" (phishing blast, mass-mail), trace
   by sender only across the window and report the recipient count — note if
   the result set hit the portal's result cap rather than presenting a capped
   number as a total.
5. Post a plain-text note: trace parameters (sender, recipient, window), trace
   type used, verdict per message, what the verdict means, and the follow-up
   action. Attach or reference the CSV for historical searches. Log time.

## Guardrails

- Never present "no results" as "they never sent it" — it means the message
  never hit this tenant's Exchange Online, which has several causes.
- State the 10-day standard-trace boundary and the async delay for historical
  searches up front; do not promise instant answers on old mail.
- If a result set may be capped, say so in the note; do not report a capped
  count as exact.
- Message content is not visible in a trace — never imply you read the email.
  If content is needed, that is a compliance search with its own authorization
  (see delegate-access-forensics / litigation-hold territory).
- Do not fabricate Message-IDs or trace rows; quote only what the tech pasted.
