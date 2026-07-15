---
name: BitLocker Key Retrieval
description: Handle "I need my BitLocker recovery key" requests — identity verification FIRST, device-ownership match, key delivered over a verified channel, key rotated after use, audit note without the key. Use whenever anyone asks for a BitLocker recovery key or a device is stuck at the blue recovery screen.
category: M365 Administration
tools: [search_tickets, search_contacts, search_knowledge_base, search_itglue, search_hudu, add_ticket_note, update_ticket, web_search]
---

# BitLocker Key Retrieval

A BitLocker recovery key unlocks the entire disk — handing it to the wrong
person is handing over the machine's data. This request is an identity
verification problem first and a console lookup second, and the key never
lands in the ticket.

## When to use

- "<user> is stuck at the BitLocker recovery screen and needs the key."
- A device prompts for recovery after a BIOS/firmware update, hardware
  change, or repeated boot failures.
- A tech needs the key for a legitimate repair/reimage on a client device.
- Data recovery from a disk pulled out of a decommissioned machine.

## Steps

1. **Verify identity FIRST — before any lookup.** This specializes the
   password/MFA recovery ladder: callback to a phone number already on file
   (search_contacts — never a number provided in the ticket itself), or the
   client's documented verification procedure (search_itglue / search_hudu /
   search_knowledge_base). Heightened scrutiny applies: a BitLocker key
   request is a known social-engineering play because the recovery screen
   "urgency" story writes itself. Requests from a third party "on behalf of"
   the user, or for a device belonging to someone else, verify with the
   device's owner or the client's IT authority, not the requester.
2. **Match requester to device.** Confirm the device (by the recovery key ID
   shown on the recovery screen — have the user read the key ID, not guess
   at device names) maps to a device record whose registered owner is the
   verified requester, or that the client authority has approved access for
   a repair scenario. Key ID matching also guarantees the tech pulls the
   right key when a user has several devices.
3. **Understand why recovery triggered.** Firmware/BIOS update, TPM change,
   hardware swap, or boot-order change are benign classics. No plausible
   trigger — or recovery prompts appearing across multiple devices at once —
   is a security signal: pause and escalate per the client's security
   process before unlocking anything.
4. **Retrieve.** Tech looks up the key in Entra (device object → BitLocker
   keys) or Intune by the key ID. If the key is absent (device never
   escrowed), say so honestly — do not improvise recovery promises; escalate
   to data-recovery options and flag the escrow gap as a finding (and check
   whether the device fell to stale-device-cleanup deletion — a process
   lesson worth recording).
5. **Deliver over a verified channel only.** Read the key to the verified
   user on the verified callback, or use the client's documented secure
   channel. Never paste the recovery key into the ticket, a note, email, or
   chat — those all sync and persist.
6. **Rotate after use.** Once the device is unlocked and healthy, the tech
   triggers a recovery-key rotation (Intune supports rotation on
   Entra-joined devices) so the disclosed key is dead. If rotation isn't
   available for this device type, record that the disclosed key remains
   valid — that residual risk belongs in the note.
7. **Audit note.** Plain-text note with: requester, verification method used
   (e.g., "callback to number on file"), device and key ID (the ID is safe;
   the key is not), reason recovery triggered, delivery channel, rotation
   done or residual-risk flag, and tech who executed. The note proves the
   control worked without weakening it.

## Guardrails

- No verification, no key — regardless of seniority, urgency, or how stuck
  the machine is. VIP pressure is the attack's costume.
- The recovery key never appears in tickets, notes, email, or chat. Key ID
  yes; key no.
- A recovery prompt with no plausible hardware/firmware trigger — or several
  at once across a client — is treated as a potential security event before
  it is treated as a support task.
- Key rotation after disclosure is part of the job, not an optional cleanup.
- Absent escrow is reported honestly and logged as a finding; never imply
  the data is recoverable when the key does not exist.
