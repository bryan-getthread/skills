---
name: Mobile Device & MDM
description: Work mobile device tickets — enrollment failures, mail profiles not arriving, compliance blocks, and lost/stolen device response — with all destructive device actions gated behind explicit approval.
category: Troubleshooting Playbooks
tools: [search_tickets, search_knowledge_base, search_itglue, search_hudu, send_approval, add_ticket_note, web_search]
---

# Mobile Device & MDM

Phones and tablets fail in three places — enrollment, the profiles pushed to them, and access policy — plus the one scenario with a clock on it: a lost or stolen device. This playbook branches those cleanly and treats every remote lock/wipe as an approval-gated action, never a reflex.

## When to use

- "<user>'s phone won't enroll" / "company portal errors out"
- Work email/Wi-Fi/apps never arrived on an enrolled device
- Device blocked from mail or apps by compliance
- "<user> lost their phone" or a device was stolen

## Steps

1. **History first.** search_tickets for this user/device and this client's MDM. Several enrollment failures this week → a tenant-side cause (expired push certificate/token is the classic — it breaks everyone at once), not per-device fixes.
2. **Docs second.** search_itglue / search_hudu / search_knowledge_base for the client's mobile standard: MDM product, BYOD vs corporate-owned enrollment types, which profiles are pushed (mail, Wi-Fi, VPN), compliance rules, and the lost-device procedure if one is documented.
3. **Identify versions — never assume.** Device platform and OS version, and the enrollment/portal app version. Minimum-OS compliance rules and platform changes routinely explain "it worked on the old phone."
4. **Get the error before theorizing.** The exact enrollment error text/screenshot, or the compliance state and reason shown in the MDM console for the device. The console's stated reason beats any endpoint guesswork.
5. **Branch:**
   1. **Enrollment failures** — check in order: tenant health (push certificates/tokens current — expired Apple push cert breaks all iOS management at once; escalate to the MDM admin immediately and say only re-issuing it fixes anything); user licensing/eligibility and enrollment restrictions (device type or personal-device caps); stale device records (a previously enrolled device must often be deleted from the console before re-enrolling — follow the MDM's procedure); device-side basics (network reachability to the platform's enrollment endpoints, date/time correct). Corporate-owned automated enrollment (ABM/zero-touch) failing → check the serial's assignment to the right MDM server before anything else.
   2. **Profile pushes (mail/Wi-Fi/VPN not arriving)** — confirm the device is actually enrolled and syncing (last check-in time), the profile is assigned to this user/device group, and deployment status/errors in the console for that profile. A profile that requires prerequisites (mail needs the account's licensing; Wi-Fi cert needs the cert profile) fails silently when its dependency does — check the chain. Guide a manual sync from the device as the cheap first test. Escalate when: the profile errors reference tenant configuration (broken cert connectors, misconfigured payloads) — MDM admin territory.
   3. **Compliance/access blocks** — the console names the failed rule (OS below minimum, jailbreak/root detection, encryption off, inactivity). Fix the device against the rule, or route genuine business exceptions to the policy owner — never suggest weakening the compliance policy as troubleshooting.
   4. **Lost or stolen device — time matters.** Confirm identity of the reporter by callback to a number on file (not a number from the ticket). Establish: last seen, corporate vs BYOD, and what data classes the device could access. Actions in escalating order — locate, remote lock, retire/selective wipe (removes work data only; right default for BYOD), full wipe (corporate device, or theft with data exposure). Every action beyond locating goes through send_approval to the authorized client contact with the action, the device, and consequences stated plainly — no lock or wipe without recorded approval, except where the client's documented procedure pre-authorizes it. Also trigger: account credential resets and session revocation (the phone may hold tokens) — pair with the M365 sign-in guidance; and flag security review if company data exposure is plausible.
6. **Verify and note.** Enrollment: device green in console with profiles delivered and mail flowing. Lost device: action confirmed in console, approvals recorded, credential resets done. Plain-text note: branch, console evidence, actions and approvals, verification.

## Guardrails

- Remote lock, retire, and wipe are approval-gated via send_approval — full wipe on a BYOD device is a legal/data-loss landmine: default to selective wipe and require explicit approval to exceed it.
- Identity verification by callback to a number on file before any lost-device action or credential reset — never trust contact details supplied in the ticket itself.
- Never advise loosening compliance policy or enrollment restrictions as a fix — route exceptions to the policy owner.
- Tenant-level failures (push certs, tokens, connectors) can only be fixed by the MDM/Apple/Google admin path — say so and escalate rather than burning time on devices.
- No remote execution by the agent — console actions are performed by the tech; this playbook supplies the sequence and the gates.
- Docs tools vary per tenant — note what you could not check, especially whether a documented lost-device procedure exists.
