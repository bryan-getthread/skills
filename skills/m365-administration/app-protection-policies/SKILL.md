---
name: App Protection Policies
description: Set up MAM-without-enrollment for BYOD — what app protection actually protects (org data in managed apps) and doesn't (the device), with the user-experience honesty that makes BYOD adoption work. Use for "protect company mail on personal phones", MAM policy requests, or "can we wipe company data without touching their photos".
category: M365 Administration
tools: [search_tickets, search_knowledge_base, search_itglue, search_hudu, add_ticket_note, update_ticket, send_approval, schedule_ticket, web_search]
---

# App Protection Policies

App protection (MAM-WE) draws a container around org data inside managed
apps on a device the MSP does not manage. Its adoption succeeds or fails on
one honest sentence to users: "the company can see and wipe its data in work
apps — not your photos, messages, or location." This skill configures the
container and delivers that sentence.

## When to use

- "Company email on personal phones needs protection, but users refuse MDM."
- "If <user> leaves, can we remove company data from their personal phone?"
- BYOD population created by a personal-device enrollment block (see
  enrollment-restrictions).
- "What can the company actually see on my phone?" — the trust question.

## Steps

1. **Scope and licensing.** Confirm Intune licensing covers the target users
   and identify the BYOD population (which users, which platforms). Docs
   first: search_itglue / search_hudu / search_knowledge_base for the
   client's BYOD stance and any existing MAM policy to extend rather than
   duplicate.
2. **Be precise about what it protects — and put it in every artifact:**
   - **Protects:** org data inside policy-managed apps (Outlook, Teams,
     OneDrive, Office...) — PIN/biometric to open work apps, encryption of
     org data, cut/copy/paste and save-as restrictions to unmanaged apps,
     and selective wipe of org data only.
   - **Does not:** manage the device, see personal apps/photos/texts/
     browsing/location, patch the OS, or protect data in apps outside the
     policy. It also does not make a compromised device safe — pair with
     conditional launch checks (min OS, jailbreak/root block) for a floor.
   Plans or client comms that blur this line get corrected before they ship.
3. **Design the policy:** target the managed-app set (start with the
   Microsoft core apps in use), data-transfer rules (org data to
   policy-managed apps only; decide contact-sync and backup exceptions
   deliberately — over-tight paste/save rules are the #1 usability
   complaint), access requirements (app PIN, biometric allowed), and
   conditional launch (minimum OS, jailbreak/root block, offline grace
   period, wipe-on conditions).
4. **Enforce the gate or it's optional.** MAM without a Conditional Access
   policy granting mail/app access only to protected apps is opt-in
   security — users on unprotected native mail clients bypass it entirely.
   Plan the CA "require app protection policy" grant alongside the MAM
   policy, with report-only soak per the conditional-access-review
   discipline, and state the user-visible effect: native/unsupported mail
   apps will stop working; Outlook (etc.) becomes the path.
5. **User experience honesty in the comms:** what changes (app PIN prompt,
   work data stays in work apps, native mail stops if CA enforces), what the
   company can do (wipe org data from work apps), and what it cannot see or
   touch (personal content, location). This is the adoption make-or-break —
   BYOD users who suspect surveillance refuse, escalate, or quietly stop
   reading mail off-hours.
6. **Approval and pilot.** send_approval to the client authority: policy
   settings, the CA enforcement decision and its user-visible effects,
   comms, pilot group, and rollback (unassign policy; relax the CA grant —
   both reversible without touching devices). Pilot on real personal
   devices across platforms; verify the block behaviors AND that legitimate
   daily flows survive before broadening (schedule_ticket for rings).
7. **Offboarding tie-in.** Record in the client's documentation that a
   departing BYOD user gets a selective wipe (org data only) as part of
   offboarding — this is the payoff of the whole setup; wire it into the
   client's offboarding checklist (see employee-offboarding).

## Guardrails

- Never describe MAM as "managing" or "securing the phone" — it protects
  org data in managed apps, and every artifact says exactly that.
- No selective wipe without the standard destructive-action approval; even
  "org data only" is user-visible and occasionally catches unsaved work.
- MAM without CA enforcement is labeled "optional protection" in the plan —
  the client must choose enforcement (or its absence) knowingly.
- Do not tighten data-transfer rules past what the pilot proves usable;
  a policy users route around protects nothing.
- Personal-device privacy claims must be accurate for the platforms in
  question — verify current platform behavior (web_search / vendor docs)
  rather than asserting from memory.
