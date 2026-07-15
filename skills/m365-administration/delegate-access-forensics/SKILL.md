---
name: Delegate Access Forensics
description: Answer "who sent/deleted/read what as whom" from the mailbox audit log — Send As vs Send on Behalf vs owner actions distinguished, logon types read correctly, findings reported neutrally. Use for delegation disputes, "I never sent that email," or "someone deleted messages from my mailbox."
category: M365 Administration
tools: [search_tickets, search_contacts, search_clients, add_ticket_note, update_ticket, log_time_entry, web_search]
---

# Delegate Access Forensics

Resolves a delegation dispute with audit evidence instead of accusations:
which identity performed the action, under which access type, when — and an
honest statement of what the log cannot prove.

## When to use

- "An email went out from my address and I didn't send it."
- "Messages are disappearing from the shared mailbox — who's deleting them?"
- "Did the assistant read/move the manager's mail?"
- A mailbox-permissions-audit finding needs "was it USED?" answered.
- If the pattern suggests compromise rather than a known delegate, the
  security runbooks (compromised-account-containment, account-takeover-runbook)
  lead; this skill supplies the mailbox-audit evidence.

## Steps

1. Frame the question precisely: which mailbox, which action (send, delete,
   move, read), what time window, and which suspected actor if any. Confirm
   the requester is authorized to ask — mailbox audit data about a person is
   sensitive; the mailbox owner, their management, or the client's documented
   authority, per policy. HR/legal-flavored requests get the client's
   authority on record before any search.
2. Check the evidence window before promising results: mailbox auditing is on
   by default in Exchange Online (since 2019), and audit retention is limited
   (180 days standard at current writing — verify the tenant's actual
   retention and license tier; E5/add-ons extend it). If the event predates
   retention, say so up front: no log, no finding.
3. Prepare the search for the tech (verify against current module versions):
   `Search-UnifiedAuditLog -StartDate <t1> -EndDate <t2> -RecordType
   ExchangeItem -ObjectIds/-FreeText <refs>` or Purview Audit search scoped to
   the mailbox. Filter to the operations that answer the question:
   - Sending: `SendAs`, `SendOnBehalf` (delegate sends) vs `Send` (owner).
   - Deleting: `SoftDelete`, `HardDelete`, `MoveToDeletedItems` — these are
     three different user behaviors; report which one actually occurred.
   - Reading/moving: `MailItemsAccessed` (license-dependent), `Move`,
     `Update`, `FolderBind`.
4. Read each hit's identity fields carefully — this is where forensics goes
   wrong: `UserId`/`UserKey` is the acting identity; `LogonType`
   distinguishes Owner vs Delegate vs Admin access; `ClientInfoString`/
   `ClientIP` show the client and source. A `SendAs` by delegate-with-
   permission is a very different finding from a `Send` by the owner's own
   session at a foreign IP (that's compromise — reroute to security).
5. Correlate: match audit rows to the disputed items (subject, timestamps,
   Message-IDs from mail-trace-investigation where sends are disputed), and
   pull the current permission state (mailbox-permissions-audit) to establish
   whether the actor's access was granted, when, and by whom.
6. Report neutrally and completely: what the log shows (identity, access
   type, operation, timestamp, client/IP), what it does NOT show (audit logs
   don't capture content; `MailItemsAccessed` coverage depends on licensing;
   gaps are stated), and the confidence level. Never write "X read the CEO's
   mail" when the evidence is "a FolderBind by X's identity under Delegate
   logon."
7. Post a plain-text note: question asked, authorization on record, search
   parameters, findings table (operation, actor, logon type, timestamp,
   client), explicit gaps/caveats, and recommended follow-up (revoke a grant
   via shared-mailbox-delegation, security escalation, or no action). Export
   the raw results as CSV evidence referenced from the note. Log time.

## Guardrails

- No search without confirmed authorization; investigating a person's
  mailbox activity is an HR/legal-adjacent act.
- Findings are stated as what the log records, never as conclusions about
  intent; disputed-conduct wording stays neutral and factual.
- State retention and licensing limits honestly — absence of a log entry
  within a covered window is evidence; absence outside it is nothing.
- Signs of external compromise reroute to the security runbooks before any
  permission is touched — don't tip the attacker.
- Do not fabricate or infer audit rows; report only what the tech's search
  returned, and mark result-cap truncation if the search hit limits.
