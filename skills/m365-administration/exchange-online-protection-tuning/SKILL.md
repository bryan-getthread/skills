---
name: Exchange Online Protection Tuning
description: Tune Exchange Online Protection mail-flow hygiene from evidence — inbound/outbound connectors, disciplined allow/block lists, and spoof-intelligence handling — without punching holes in the filter. Use when a client has connector/mail-routing issues, wants to review EOP allow/block entries, or asks about spoofed-sender handling.
category: M365 Administration
tools: [search_tickets, search_clients, search_knowledge_base, search_itglue, search_hudu, add_ticket_note, send_approval, log_time_entry, web_search]
---

# Exchange Online Protection Tuning

Tunes the EOP layer that sits in front of every mailbox: connectors that route
mail in and out, the Tenant Allow/Block List, and spoof intelligence. The
theme is discipline — allow/block lists only ever grow unless someone owns
them, and a sloppy connector or a domain-wide allow is a spoofing highway.

## When to use

- "Mail from our line-of-business app / smart host isn't delivering" (connector).
- "Review our allow/block list — it's a mess."
- "A spoofed sender got through" or "our own domain is being spoofed."
- Hardening EOP mail-flow posture after an incident.
- For content-filter thresholds (spam/bulk verdicts) see
  anti-spam-policy-tuning; this skill is connectors + allow/block + spoof.

## Steps

1. Diagnose from evidence. Pull message traces (mail-trace-investigation) and,
   for spoof issues, headers (email-header-analysis: SPF/DKIM/DMARC and the
   spoof verdict). Tune against the reason the trace/header shows, not the
   complaint.
2. Connectors: for inbound app/smart-host or partner routing, verify the
   connector's scope (sender domain/IP), TLS/certificate requirements, and
   that it isn't broader than it needs to be. An overly broad inbound
   connector that trusts a wide IP range can bypass filtering — scope it to
   the specific sender. Outbound connectors to a partner/host: confirm they
   don't inadvertently route mail that should stay in EOP. Document any
   connector change carefully; connectors break mail flow when wrong.
3. Allow/Block list discipline. Use the Tenant Allow/Block List for specific
   senders, spoofed pairs, URLs, or file hashes — time-limited where the
   portal supports it, with a review date always. Refuse broad allows: never
   allow the client's own domain, a free-mail domain, or a whole vendor
   domain — those are the canonical spoofing holes. When asked to "just
   whitelist the domain," offer the scoped sender/pair alternative and explain
   why. Audit the existing list for stale, broad, or unexplained entries and
   propose pruning.
4. Spoof intelligence: review the spoof-intelligence insight for senders
   spoofing the client's domain. Legitimate spoofers (a marketing platform
   sending as the client) get an explicit allowed spoofed-pair entry AND a
   nudge to fix it properly at the source (SPF/DKIM/DMARC —
   dmarc-spf-failure-triage). Malicious spoofers stay blocked. Don't blanket-
   allow spoofing to silence a few false positives.
5. Approval: connector changes and allow/block entries alter delivery and
   security posture — get client sign-off (send_approval) with the evidence,
   the exact change, and the scope/expiry of any entry.
6. Prepare execution for the tech (verify against current portals): Exchange
   admin center (Mail flow > Connectors) or `New-/Set-InboundConnector` /
   `OutboundConnector`; `New-TenantAllowBlockListItems` and the Defender
   portal for allow/block and spoof intelligence.
7. Verify with evidence: the affected mail routes/filters as intended after
   the change and ONLY the intended mail is affected; re-trace to confirm.
   Post a plain-text note: evidence (trace/header refs, spoof verdicts),
   connector or list change and its scope, review/expiry date, approver, date,
   and rollback (restore prior connector config / remove entry — prior values
   captured first). Log time.

## Guardrails

- No connector or allow/block change without trace/header evidence.
- Scope connectors to the specific sender/IP; a broad trusted connector
  bypasses filtering.
- Never allow the client's own domain, free-mail domains, or whole vendor
  domains; offer scoped sender/spoofed-pair entries instead. Every entry
  carries a review date.
- Fix legitimate spoofing at the source (SPF/DKIM/DMARC), don't paper over it
  with blanket allows.
- Connectors break mail flow when misconfigured — capture prior config as
  rollback and change deliberately, with client approval.
