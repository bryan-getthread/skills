---
name: M365 Sign-in Issues
description: Diagnose Microsoft 365 / Entra sign-in failures — blocked sign-ins, MFA loops, "keeps asking for password", device-trust errors — starting from the sign-in log error code, never from guesswork.
category: Troubleshooting Playbooks
tools: [search_tickets, search_knowledge_base, search_itglue, search_hudu, add_ticket_note, web_search]
---

# M365 Sign-in Issues

The rule of this playbook: get the AADSTS error code from the Entra sign-in logs before proposing anything. The code, plus the Conditional Access tab of that sign-in event, usually names the cause outright.

## When to use

- "<user> can't sign in to M365 / Outlook / Teams" with or without an error
- MFA prompt loops, "approve sign-in" never arrives, or authenticator out of sync
- "You can't get there from here" / device compliance or trust errors
- Password accepted but apps keep re-prompting

## Steps

1. **History first.** search_tickets for this user and for the same error across the client. One user → account/device path. Many users starting at the same time → tenant-wide change (CA policy edit, license, federation) — treat as an incident and find what changed.
2. **Docs second.** search_itglue / search_hudu / search_knowledge_base for the client's identity setup: cloud-only vs hybrid, MFA method standard, named CA policies, device join type expected (Entra joined / hybrid / registered).
3. **Get the log before theorizing.** Guide the tech to Entra admin center → Sign-in logs → the failing event. Capture: AADSTS error code, failure reason, Conditional Access tab result, device info, and location/IP. Do not proceed on a screenshot of the client-side dialog alone — the server-side event is the truth.
4. **Decode the code.** Look up the exact AADSTS code (web_search against Microsoft's documented list if not recognized). Do not paraphrase from memory — codes are specific.
5. **Branch:**
   1. **Conditional Access hit** — sign-in log shows a named policy = Failure. Read which control failed: location, device compliance, app, or MFA. If the user is legitimately outside policy (new location, unenrolled device), the fix is bringing them into policy, not weakening it. Escalate when: a recently edited policy is blocking a class of users — route to the identity owner with the policy name and evidence; never edit CA policy as a troubleshooting step.
   2. **MFA loop / prompt fatigue** — repeated prompts or approvals that never land. Check the registered methods for stale devices (old phone still primary), time-drift on TOTP, and whether a CA policy demands a method the user lacks. Re-registration of methods is an identity-verified action — verify the person by callback to a number on file, never a number supplied in the ticket.
   3. **Token / "keeps asking for password"** — auth succeeds in logs but apps re-prompt. Usually stale cached credentials or broken token refresh on one device. Guide: sign out of all Office apps, clear stored credentials for the Microsoft identity in the OS credential store, sign in fresh. If only one app misbehaves, pair with that app's playbook (Outlook, Teams).
   4. **Device trust / compliance** — error mentions device state, or CA requires compliant/joined device. Guide the tech to run `dsregcmd /status` on the device and read AzureAdJoined / DomainJoined / DeviceAuthStatus, and check the device record in Entra and the MDM compliance state. Hybrid-join failures are frequently sync-related — check the last Entra Connect sync. Escalate when: sync or PRT issues affect multiple devices — identity/infra owner, not per-device fixes.
   5. **Account state** — disabled account, expired password, risk-based block (sign-in risk policy). If risk-flagged: do not simply unblock — confirm with the user what happened and pair with the security playbooks if the sign-in wasn't theirs.
6. **Verify and note.** A fresh successful sign-in event in the logs is the proof, not the user's "seems ok". Plain-text note: AADSTS code, CA result, branch, action taken/handed off, verification event time.

## Guardrails

- Never propose disabling MFA, excluding a user from Conditional Access, or relaxing a policy as a "fix". Bring the user into compliance, or escalate the policy question to its owner.
- Identity verification before any credential/MFA reset: callback to a number on file — never one provided in the ticket thread.
- No remote execution — dsregcmd and sign-out steps are guidance for the tech or the user.
- Do not paraphrase AADSTS codes from memory; verify the exact meaning before acting on it.
- If the failure is inside Microsoft's service (service health incident), say only Microsoft can act, reference the incident, and set expectations honestly.
- Docs tools vary per tenant — note anything you could not check.
