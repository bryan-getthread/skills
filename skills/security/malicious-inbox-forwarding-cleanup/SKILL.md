---
name: Malicious Inbox Forwarding Cleanup
description: The remediation runbook for a mailbox found with attacker-planted rules and forwarding — inventory every rule, forward, delegate, and send-as, remove the malicious ones, and confirm mail flow is clean.
category: Security
tools: [search_tickets, search_contacts, add_ticket_note, update_ticket]
---

# Malicious Inbox Forwarding Cleanup

The standalone cleanup half of a takeover: once you know (or strongly suspect) a mailbox is compromised, this is the disciplined sweep that removes the attacker's mail-based persistence and proves it's gone. Where inbox-rule-alert-runbook decides whether a single alerted rule is malicious, this skill assumes malicious and does the full inventory-and-remove across every hiding place.

## When to use

- A mailbox is confirmed/suspected compromised and needs its rules and forwarding cleaned (called from account-takeover-runbook, business-email-compromise-recovery, session-token-theft-response)
- inbox-rule-alert-runbook reached a malicious verdict and hands off to full cleanup
- A user reports mail "disappearing," replies they never see, or a colleague getting their mail — do the full sweep, not a one-rule fix

## Steps

1. Inventory ALL of it before removing anything — attackers rarely stop at one artifact, and one visible rule is often bait for a quieter one. Enumerate: inbox rules (all of them, including disabled), account-level forwarding (SMTP forward / redirect), mailbox delegates and full-access grants, and "send-as" / "send-on-behalf" permissions.
2. Flag the malicious markers: rules named ".", "..", or a single character; actions that hide mail (move to RSS Feeds, Deleted Items, Archive, Conversation History, or mark-as-read); conditions keyed on invoice, payment, password, security, wire, or helpdesk; auto-forward to any external address; delegates or forwards the user doesn't recognize.
3. Capture evidence first: record each malicious rule/forward/delegate's full definition and creation timestamp into the ticket BEFORE removing it. Removal destroys the artifact — preserve it for the record and any downstream notification.
4. Remove the malicious entries: delete the rules, disable external forwarding, remove unrecognized delegates and send-as grants. Leave the user's legitimate rules alone — confirm ambiguous ones with the user via a verified channel (a number on file, not an address from the ticket).
5. Verify clean state: re-list rules, forwarding, and delegates after removal to confirm nothing regenerated and nothing was missed. Check tenant-level transport rules too if the client's admin reports mail still going astray — a mailbox rule is not the only place mail can be redirected.
6. Confirm this is part of a full response, not the whole of it: removing rules without rotating the password and revoking sessions/tokens leaves the attacker able to re-plant. If containment hasn't happened, return to account-takeover-runbook. Removal and rotation travel together.
7. Document the decision, not just the action: every artifact found (with its definition and timestamp), what was removed, what was confirmed legitimate and by whom, and the post-cleanup verification. Classify per soc-classification-tree.

## Guardrails

- Cleanup without containment is not remediation — if the password isn't rotated and sessions/tokens aren't revoked, the attacker re-plants. Removal and rotation travel together.
- Inventory all five hiding places (rules, forwarding, delegates, send-as, tenant transport rules) — one cleared rule is not a cleaned mailbox.
- Preserve each artifact's definition before deleting it; the evidence matters for the record and for warning anyone the forward leaked to.
- Don't delete the user's legitimate rules — confirm the ambiguous ones via a verified channel; malicious markers outvote a vague "that's probably mine."
- Rule/forwarding/delegate changes are executed by the client's admin or through the supported mailbox tooling — the agent drives and documents.
- Defensive writing: "a mail-forwarding rule was detected and removed," not "you were hacked."
- Plain text only for PSA-synced notes.
- When in doubt, sweep wider and verify again — a re-checked mailbox is cheap; a missed second rule is an open channel.
