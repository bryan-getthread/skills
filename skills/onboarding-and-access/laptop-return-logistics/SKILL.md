---
name: Laptop Return Logistics
description: Get a company device back from a departing or remote user — coordinate the carrier return box and prepaid label, send the templated return email with a deadline, confirm receipt, verify the wipe, update the asset record, then close. Includes the escalation path when the user goes silent.
category: Onboarding & Access
tools: [search_tickets, search_contacts, search_itglue, search_hudu, add_ticket_note, update_ticket, schedule_ticket, view_openDraft]
---

# Laptop Return Logistics

The offboarding is done, the accounts are disabled — and the laptop is still on someone's kitchen table three states away. This skill runs the return as a tracked sequence with dates on everything: box out, label used, device received, device wiped, asset record updated, ticket closed. Nothing is marked done until it verifiably happened.

## When to use

- An offboarding ticket (see employee-offboarding) reaches the "recover the device" step for a remote or departed user.
- "Get <user>'s laptop back" / "send a return box to <user>".
- A device recovery has stalled and someone asks where it stands.

## Steps

1. Establish the facts on the ticket (`search_tickets`, `search_contacts`): who has the device, their current shipping address and personal contact info (post-departure, their work email may be dead — confirm a reachable address; the HR/client contact on the ticket is the fallback), which device it is (asset tag/serial from the asset record in IT Glue/Hudu via `search_itglue`/`search_hudu`), and the return deadline per the client's policy.
2. Arrange the return kit: order the carrier return box with prepaid label per the desk's process (carrier portal or shipping vendor — this is an action the desk performs; record the order confirmation and tracking number on the ticket when done — never note the box as ordered before it is).
3. Send the templated return email to the user (draft via `view_openDraft` where available; otherwise present the draft for a human to send): what to return (device + charger + accessories, identified by asset tag), what is coming (return box with prepaid label, expected arrival), how to pack it, the return deadline, and who to contact with questions. Keep the tone neutral and procedural — this person may be a recent involuntary departure; no accusations, no legal threats (remedies for non-return are the client's call, not the desk's email).
4. Track the sequence with dated plain-text notes via `add_ticket_note`, and set follow-up checkpoints with `schedule_ticket`: box delivered to user (carrier tracking), label used / package in transit, package received at the destination.
5. On receipt: physically confirm the right device came back (asset tag/serial match against the record) and note condition and included accessories.
6. Wipe verification: the wipe itself runs through the desk's device-wipe process — device-wipe-workflows (m365-administration) covers choosing and gating the right Intune action. This skill's job is verification: confirm the wipe action reports completed for THIS device before recording it, and note the completion evidence. If the device was already remote-wiped pre-return, verify that status rather than re-running anything.
7. Update the asset record (IT Glue/Hudu per desk convention): status returned/in-stock, location, condition, wipe-verified date. If the docs tools are read-only for the tenant, note the exact record changes needed and route to whoever maintains the asset system — do not mark the record updated when only a request was made.
8. Close the ticket via `update_ticket` only when all of: device received and identity-confirmed, wipe verified, asset record updated (or update explicitly handed off). The closure note lists each with its date.

## Silent-user escalation path

- No response / box unused by checkpoint: send a dated reminder (attempt 2), then a final notice (attempt 3) stating the deadline and that non-return will be escalated to <client contact> — the three-strikes pattern (see three-strikes-final-email), each attempt documented on the ticket.
- After three documented attempts with no return: hand off to the client's HR/management contact with the full attempt timeline. Device recovery from an unresponsive ex-employee is the employer's action (payroll deduction, legal, police report are their remedies — never threatened by the desk).
- In parallel, confirm the device is remotely locked/wiped per the offboarding's security posture so a never-returned device is not a data risk. The ticket then tracks the asset loss, not a data exposure.

## Guardrails

- Zero-assumption rule throughout: box ordered, email sent, package received, wipe completed, asset updated — each is recorded only after it verifiably happened, with a date. Never convert "should happen" into "done".
- Wipe before closure is non-negotiable when the device contained company data; "it came back" is not "it is clean". Destructive wipe decisions and approvals belong to device-wipe-workflows — do not shortcut its gates here.
- Verify the returned device is the right device (asset tag/serial) — the wrong or partial return (no charger, personal device swapped in) keeps the ticket open.
- Communications to a departed employee are procedural and blameless; escalation beyond reminders goes through the client, not at the user.
- Personal addresses and contact details gathered for shipping are used for shipping only — keep them out of client-visible notes (see psa-note-visibility-rules).
- Notes are plain text and dated; this ticket is the audit trail if the return ever becomes a dispute.
