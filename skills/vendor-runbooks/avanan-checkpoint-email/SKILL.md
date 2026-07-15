---
name: Avanan Check Point Email
description: An Avanan / Check Point Harmony Email alert or restore request arrived — work the inline/API-based detection (message may have been pulled post-delivery) and apply release discipline to restore requests.
category: Vendor Runbooks
tools: [search_tickets, search_contacts, add_ticket_note, update_ticket]
---

# Avanan Check Point Email

Vendor specialization of quarantine-release-request and phishing-triage for Avanan (Check Point Harmony Email & Collaboration). Avanan sits inside the mailbox via API rather than in front of it as a gateway, so its distinctive events are post-delivery: messages quarantined out of the inbox after arrival, and restore requests when users want them back. Verify plan capabilities and console paths against Check Point's current documentation.

## When to use

- An Avanan phishing/malware alert lands (message quarantined inline or retracted post-delivery)
- A user asks to restore a message Avanan quarantined out of their mailbox
- A tech asks whether an Avanan detection needs user follow-up

## Steps

1. Parse the alert anatomy: verdict (phishing, malware, spam, suspicious), detection timing — pre-delivery/inline (user never saw it) vs post-delivery retraction (the message sat in the inbox for some window before Avanan pulled it) — recipient(s), sender, and the indicators the engine cited.
2. The post-delivery window is the vendor-specific risk: if the message was readable before retraction, assume possible interaction. Confirm with each recipient via a verified channel whether they opened it, clicked anything, or entered credentials. Clicked/credential exposure → phishing-triage and, where credentials are plausible, compromised-account-containment. Pre-delivery quarantines with no exposure window can follow the normal evidence-based close.
3. Sibling scoping: one detection usually has co-recipients — the technician checks the Avanan console for the same campaign across mailboxes; the agent checks search_tickets for parallel reports. Retractions across many mailboxes at once are a campaign — work as one incident, not per-ticket.
4. Restore requests → run quarantine-release-request as the spine: verify the requester is the mailbox owner or authorized for it, read WHY Avanan quarantined it (verdict class sets the scrutiny floor — malware never restores on say-so; phishing verdict requires a phishing-triage false-positive outcome first), assess sender legitimacy without touching the payload, and restore to the requesting mailbox only. The restore itself is the technician's console action; the agent recommends and records.
5. Recurring false positives → security-noise-tuning: narrowest allow (sender over domain), named approver, review date — and note that allow-list entries in an inline tool re-expose the mailbox directly, so the bar is high.
6. Document the decision, not just the action: verdict, detection timing, exposure-window outcome per recipient, requester verification for restores, who restored what and when. Classify per soc-classification-tree; client-facing wording per defensive-writing-standard.

## Guardrails

- All quarantine-release-request guardrails apply: requester verification, no payload interaction, urgency is a caution flag, when in doubt don't restore.
- Post-delivery retraction is never closed without checking the exposure window — "Avanan removed it" answers where the message is, not what the user did with it first.
- Never restore a malware-verdict item; anyone insisting escalates to management.
- Campaign-scale retractions get incident treatment and a single evidence trail, with companion tickets merged per alert-storm-merge (exact references only).
- Console actions (restore, allow-list, campaign view) are technician steps the agent directs and records.
- Degradation: without console visibility, the agent sees only the ticket's copy — name what the tech should pull (verdict detail, co-recipients, retraction time) rather than guessing.
