---
name: Quarantine Release Request
description: Someone asked to release a quarantined email — verify the requester, assess why the filter caught it, and recommend release or refusal with the reasoning documented.
category: Security
tools: [search_tickets, search_contacts, add_ticket_note, update_ticket]
---

# Quarantine Release Request

A release request is a Request ticket with an Incident hiding inside it sometimes: the ask itself can be socially engineered, and the message was quarantined for a reason. This skill verifies both before anything gets released.

## When to use

- A user asks "can you release this email from quarantine?"
- A quarantine-digest reply or release-request ticket lands on the board
- A tech wants a second opinion before releasing a held message

## Steps

1. Verify the requester: is the request coming from the mailbox owner or a party authorized for that mailbox (search_contacts)? A third party pressing for the release of someone else's held mail — especially with urgency — is itself a social-engineering marker. Confirm through a verified channel if anything feels off.
2. Read WHY the filter held it: spam/bulk score, phishing verdict, malware verdict, spoof/authentication failure, or policy rule. The quarantine reason sets the floor for how much scrutiny the release needs.
3. Assess the message's legitimacy without interacting with its payload (no clicking links, no opening attachments): sender reputation and history with the client (search_tickets for prior correspondence or prior reports of the sender), whether the business context makes the message expected, and whether indicators from phishing-triage apply.
4. Decide by verdict class:
   - Malware verdict → never recommend release. Explain why to the requester; offer to obtain the content through a verified channel with the real sender instead.
   - Phishing verdict → run the phishing-triage assessment first; only a clear false-positive outcome supports release.
   - Spam/bulk or policy hold with a known legitimate sender and expected business context → recommend release, and note whether an allowlist adjustment is warranted (route recurring false positives to security-noise-tuning).
5. The release itself is the technician's action in the mail console — the agent recommends and records.
6. Document the decision, not just the action: requester verification, quarantine reason, evidence weighed, verdict, and who released what when. Classify as a Request per soc-classification-tree.

## Guardrails

- Never recommend releasing a malware-verdict item — no exceptions inside this skill; anyone insisting escalates to management.
- Never interact with the held message's links or attachments during assessment.
- Urgency from the requester is a caution flag, not a reason to skip verification — pressure is how bad releases happen.
- When in doubt, don't release — escalate. A delayed newsletter is cheap; a released payload isn't.
- Recommend-and-document is the boundary: the agent does not perform releases.

## Unattended (Flows) variant

- Follows the Unattended Output Discipline contract: the entire reply is a plain-text assessment note posted verbatim — requester-match from records, quarantine reason, evidence weighed, recommendation. No narration.
- Unattended recommendations are asymmetric: "do not release" and "needs human review" are the ONLY recommendations this variant may post. Recommending release requires verifying the requester through a verified channel — a judgment step that stays attended. Malware verdict → the note states refusal per policy, always.
- Deterministic inputs from the flow: the release-request ticket id. Quarantine reason not readable from the thread → output nothing.
- Requester does not match the mailbox owner in contact records → the note flags a possible social-engineering marker and routes to human review; when in doubt, that is the default outcome.
- Permitted writes: `add_ticket_note` only. No status or priority changes, no client-facing replies, and never any interaction with the held message's links or attachments.
