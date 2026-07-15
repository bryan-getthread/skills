---
name: Mail Forwarding Audit
description: Inventory every forwarding path in a tenant or mailbox — mailbox-level forwarding, inbox rules, and transport rules — and treat external forwarding as the security surface it is. Use when a ticket asks "is anything forwarding out," a security review needs a forwarding sweep, or mail is arriving somewhere it shouldn't.
category: M365 Administration
tools: [search_tickets, search_contacts, search_clients, search_itglue, add_ticket_note, update_ticket, log_time_entry, web_search]
---

# Mail Forwarding Audit

Finds all three layers of forwarding — because auditing one layer while mail
leaks through another is how exfiltration survives a "we checked" — and
classifies every forward as sanctioned or suspect.

## When to use

- Periodic security review: "audit all forwarding for <client>."
- "Copies of my mail are going somewhere" / mail appearing at an external
  address.
- Post-compromise sweep (attacker-added forwards are a top persistence move —
  coordinate with compromised-account-containment when that's the context).
- Before a migration (forwards are one of the things that silently break —
  cross-ref mailbox-migration-prep).

## Steps

1. Scope: one mailbox or tenant-wide. Then collect all three layers — have
   the tech run and paste (verify against current module versions):
   - Layer 1, mailbox-level: `Get-Mailbox -ResultSize Unlimited | Where
     {$_.ForwardingAddress -or $_.ForwardingSmtpAddress} | Select Name,
     ForwardingAddress, ForwardingSmtpAddress, DeliverToMailboxAndForward`.
     Note: ForwardingSmtpAddress (user/admin-set, external-capable) and
     ForwardingAddress (admin-set, directory object) are different fields —
     collect both.
   - Layer 2, inbox rules: `Get-InboxRule -Mailbox <user>` per mailbox,
     filtering for ForwardTo, ForwardAsAttachmentTo, RedirectTo actions.
     Tenant-wide this is a loop and slow — say so and cap expectations.
     Include disabled rules and rules with blank/whitespace names in the
     review (both are attacker tells).
   - Layer 3, transport rules: `Get-TransportRule` filtered for redirect,
     BCC, and "add recipient" actions (cross-ref transport-rule-management).
   - Also pull the Defender "auto-forwarded messages" report if available —
     it catches forwards that actually fired, not just configured ones.
2. Classify each forward:
   - Internal target, documented reason (coverage, shared workflow) →
     sanctioned; confirm it's still needed if old.
   - External target → elevated scrutiny, always. Sanctioned external
     forwards (personal-domain executives, a client's parent company) must
     have documentation (search_itglue, prior tickets); everything else is
     a finding.
   - Any forward on a mailbox with recent security events, created recently
     without a ticket trail, or targeting a free-mail domain → treat as a
     possible compromise indicator and route to the security runbooks
     (inbox-rule-alert-runbook / compromised-account-containment) rather
     than quietly removing it.
3. Check the policy backstop: is external forwarding even allowed by the
   outbound spam policy (Automatic forwarding: On/Off/System-controlled)?
   If external forwards exist AND the policy allows them broadly, recommend
   tightening the policy to off-by-default with scoped exceptions — that is
   the durable fix, not whack-a-mole rule removal.
4. Removals are changes: each suspect forward gets a recommendation
   (remove / confirm with user / investigate), and removal happens with
   approval and its own note — a "suspect" forward is occasionally a
   business-critical workflow nobody documented.
5. Post a plain-text note: scope, collection date, the three layers actually
   collected (state any layer skipped or capped), full inventory with
   classification, external forwards highlighted, the outbound-policy state,
   and recommended actions. Log time.

## Guardrails

- All three layers, every time. A forwarding audit that skipped inbox rules
  is not an audit; if a layer was skipped, the note says so explicitly.
- External forwards are findings until documentation says otherwise.
- Compromise-pattern forwards go to the security playbooks first — removing
  the forward before containment tips off the attacker and destroys evidence.
- Never silently remove a forward, even an ugly one, without approval — it
  may be a live business process.
- Result caps and slow tenant-wide loops are stated honestly, not glossed.
