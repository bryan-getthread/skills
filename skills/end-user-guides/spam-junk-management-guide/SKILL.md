---
name: Spam and Junk Management Guide
description: Draft reply-ready instructions for an end user to mark junk or safely release a legitimate message from the Junk folder — NOT the phishing path — "tell the user how to handle junk mail / get a real email out of Junk."
category: End-User Guides
tools: [search_tickets, search_knowledge_base, search_itglue, search_hudu, view_openDraft]
---

# Spam and Junk Management Guide

Produces a client-ready instruction block for everyday junk-mail management — marking unwanted-but-harmless mail as junk, and safely releasing a legitimate message that landed in Junk — kept strictly separate from the phishing/suspicious-email path, which is a security workflow, not a preference tweak.

## When to use

- "User is getting marketing junk — send them how to mark it as junk / unsubscribe safely."
- "A real email from a client went to Junk — send the user how to get it out and stop it recurring."
- "How do I stop these newsletters?" everyday-noise tickets.

## Steps

1. **First, screen out the phishing case — this is the guardrail, not a footnote.** If the message is *suspicious* (unexpected link/attachment, credential lure, spoofed sender, urgency/payment bait), this is NOT a junk-management ticket — route to the report-phishing path and do not send "just mark it junk" or "add to safe senders" advice, which would train the user to whitelist an attacker. Only proceed here for clearly harmless-but-unwanted mail or a genuinely legitimate message caught by the filter. If unsure whether it's spam or phishing, treat it as phishing.
2. **Confirm the platform and filter model** (search_itglue / search_hudu / search_knowledge_base / search_tickets): Outlook/Exchange Online Protection is common; some clients run a separate spam gateway (Proofpoint, Barracuda, Mimecast) with its own quarantine digest and release process. Where mail is released and whether users are even allowed to self-release differs — verify. If unknown, ask the technician ONE question.
3. **Draft the instruction block** to end-user rules, one action per step with what-you'll-see cues, in the branch that matches:
   - **Marking unwanted (harmless) mail as junk:** select the message → the Junk/Report → "Junk" option (cue where it is in their Outlook) → it moves to the Junk folder and trains the filter. For newsletters they once signed up for, the cleaner fix is the legitimate Unsubscribe link at the bottom — but only for senders they recognize and trust.
   - **Releasing a legitimate message from Junk:** open the Junk Email folder → find the message → "Not junk" / "Report → Not junk" (cue the option), which moves it back to the Inbox. If the client uses a gateway quarantine, describe the digest email's "Release"/"Allow" action per their docs instead.
   - Adding a trusted sender to safe senders **only for clearly legitimate correspondents** — with the plain caution that safe-listing means their filter stops checking that address, so never safe-list anything they're unsure about.
   - The recurring-problem note: "If real emails keep landing in Junk, reply and we'll look at the filter — don't safe-list a whole domain to force it."
   - Off-ramp (mandatory): "If an email looks suspicious rather than just unwanted — unexpected links, asks for a password or payment, or seems to be pretending to be someone — don't mark it junk or unsubscribe. Report it instead — reply here and we'll guide you."
4. **Assemble per the Email Baseline Standard** (communication/email-baseline-standard) and present via view_openDraft. Do not send.

## Guardrails

- **The phishing firewall is the whole point.** This skill never handles suspicious mail — that always routes to end-user-guides/report-phishing-guide. "When in doubt, treat as phishing" is stated in every draft. Never advise safe-listing or unsubscribing a message that could be malicious.
- Safe-sender/allow-list advice is limited to clearly legitimate senders and always carries the "it stops the filter checking them" caution — never blanket-domain allow-listing by the user.
- Platform/gateway match matters: describing an Outlook "Not junk" click to a shop that releases from a Proofpoint digest sends the user to the wrong place.
- No admin steps (tenant spam policy, gateway allow/block lists, mail-flow rules, tenant-wide quarantine) in the user block — those are tech actions.
- Unsubscribe advice applies only to recognized, trusted senders — clicking Unsubscribe on true spam confirms the address is live.
- Localizable; version-cautious UI cues.
- Degradation: view_openDraft is in-app only — over external MCP, output the draft labeled "DRAFT — review before sending." Docs tools only when enabled; fall back to KB and ticket history.

## Cross-references

- end-user-guides/report-phishing-guide — the mandatory destination for any suspicious message; this guide explicitly never handles those.
- communication/email-baseline-standard.
