---
name: Apology With Remedy
description: Draft the message for when the desk genuinely failed — specific acknowledgment of what went wrong, a concrete remedy, and one prevention step — without groveling and without admissions beyond the established facts.
category: Communication
tools: [search_tickets, view_openDraft]
---

# Apology With Remedy

Drafts the accountability message that actually repairs trust: names the failure precisely, offers something real to make it right, and says what changes so it doesn't recur — in the confident voice of a professional who owns mistakes, not a supplicant.

## When to use

- "We really dropped the ball on this one — draft the apology with a make-good."
- A failure bigger than a slipped date: wrong change applied, data-affecting mistake, repeated misses on the same client, commitment broken after an escalation.
- The account manager wants an accountability message with a remedy attached, not just a sorry.

For a simple slipped timeline, use communication/delay-apology instead. If the failure surfaced as a public review, coordinate with communication/bad-review-response-prep — this skill handles the direct client message.

## Steps

1. Establish the facts from the ticket record (search_tickets): what specifically went wrong, when, what the client-visible impact was, and what has already been done to fix or contain it. The apology must match the record — apologizing for the wrong thing, or for more than what happened, both make it worse.
2. Confirm the **remedy** with the requester before drafting — it must be real, approved, and specific: a credit of a stated amount, waived hours, priority remediation work, a senior review of the affected setup. The remedy is a human decision; never invent one, never imply one is coming if none is approved. If no remedy is approved yet, tell the requester the letter is materially weaker without one and draft accordingly.
3. Confirm the **prevention step**: the one concrete change being made (a checklist added, a review gate, a monitoring alert). It must be something the desk is actually doing — a fabricated process improvement is a second failure waiting to be discovered.
4. Draft per the Email Baseline Standard skill (communication/email-baseline-standard):
   - **Specific acknowledgment, first sentence:** what went wrong and its impact on them, in plain words, no passive-voice fog ("we applied the change to the wrong server, which took your <system> offline for <duration>" — not "an issue was experienced").
   - **One apology.** Direct, unqualified — no "we apologize if this caused inconvenience," no "but."
   - **What we've done** to fix/contain it, if resolution isn't already known to the client.
   - **The remedy**, stated plainly as a done decision.
   - **The prevention step**, one or two sentences: "so this doesn't happen again, we have <change>."
   - Close forward-looking; invite a call if the relationship warrants it.
5. Tone pass — the hard part: exactly one apology, zero groveling, zero self-flagellation ("we're deeply ashamed," "unforgivable"), zero blame-shifting to vendors or tools. Confidence and ownership read as competence; excess contrition reads as instability. Cut every hedge and every excuse.
6. Present via view_openDraft, flagged for account-manager review before sending. For significant failures, recommend it come from the AM's or a leader's name.

## Guardrails

- **Facts only, admissions no further than facts.** Acknowledge what the record establishes — never speculate about additional harm, never characterize the failure legally ("negligence," "liability," "at fault"), never promise compensation beyond the approved remedy. If the incident plausibly involves legal, contractual-penalty, or insurance exposure, flag for management review before anything is sent — that is a gate, not a formality.
- The remedy and prevention step must both be real and confirmed. A hollow make-good discovered later converts one failure into two.
- One apology per message. Repetition is groveling; groveling invites renegotiation of the remedy.
- Never blame a named individual technician in client-facing text — the desk owns the failure institutionally.
- If the failure touches a security event, the Defensive Writing Standard skill (security/defensive-writing-standard) governs all wording and the incident process governs disclosure.
- Degradation: view_openDraft is in-app only — over external MCP, output in chat labeled "APOLOGY DRAFT — AM review required."
