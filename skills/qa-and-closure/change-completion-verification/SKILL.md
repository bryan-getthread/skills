---
name: Change Completion Verification
description: Verify a "change completed" claim by reading the linked ticket evidence end to end — confirm the change was actually made, was not rolled back, and that post-change validation is documented before the claim is accepted.
category: QA & Closure
tools: [search_tickets, add_ticket_note]
---

# Change Completion Verification

A tech saying "change completed" is a claim, not evidence. This skill walks the ticket (and any linked/referenced tickets) for proof the change went in, stayed in, and was validated — catching the quiet rollback that turns a completed change into a future incident.

## When to use

- "Verify this change ticket before we close it" / "was this change actually completed?"
- QA on tickets whose resolution is a change (firewall rule, DNS cut-over, migration, policy update, patch).
- A recurrence investigation where the fix was supposedly a change made weeks ago.
- Auditing a batch of "completed" change tickets before a client review.

## Steps

1. Retrieve the full ticket with `search_tickets`, including every note, time entry, and status transition in order. Identify the claimed change: what was changed, where, when, by whom.
2. Follow the evidence trail: if the ticket references other ticket numbers (a rollback ticket, a follow-up incident, a sibling change), retrieve each referenced ticket with `search_tickets` and read what happened there. A "completed" parent with a linked rollback child is NOT completed.
3. Check for rollback signals across everything read:
   - Later notes reverting, undoing, or "backing out" the change.
   - A subsequent incident on the same system that was resolved by reversing the change.
   - The change reapplied later (implies the first application did not stick).
4. Check for completion evidence, in descending strength: post-change validation documented (test result, screenshot reference, monitoring green, client confirmation of the new behavior) > a specific implementation note with timestamp and detail > a bare "done". A bare "done" with no detail is a claim, not evidence — mark it UNVERIFIED.
5. Where an RMM integration is enabled and the change touched a managed device, corroborate with device activity history (e.g. NinjaOne device activities) — a reboot, service change, or maintenance window aligning with the claimed change strengthens the evidence. Skip this step gracefully when no RMM is connected.
6. Render one of three verdicts, each citing the evidence lines: **VERIFIED COMPLETE** (implementation + validation, no rollback signals), **ROLLED BACK / NOT IN EFFECT** (rollback evidence found — the completion claim is false as of now), or **UNVERIFIED** (claim exists, evidence insufficient either way).
7. On request, post the verdict as a plain-text internal note with `add_ticket_note`: verdict, evidence cited by note date, and the missing proof if UNVERIFIED. For ROLLED BACK, recommend reopening and state which linked ticket shows the rollback.

## Guardrails

- Never accept a work summary or status flip as completion proof; the verdict rests on evidence in the source of truth. If the evidence is not there, the verdict is UNVERIFIED — never assume completion.
- UNVERIFIED is not an accusation: state exactly what documentation would resolve it (validation note, linked test, device activity reference).
- Follow linked ticket references only where the thread actually cites them; never invent ticket numbers or fabricate a rollback trail.
- Read-only by default; posting the verdict note or reopening happens only on explicit request.
- Plain text in any posted note — no markdown or emojis (PSA sync compatibility).
