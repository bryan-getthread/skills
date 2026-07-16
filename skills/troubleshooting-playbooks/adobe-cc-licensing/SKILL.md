---
name: Adobe Creative Cloud Licensing
description: Diagnose Adobe Creative Cloud licensing and sign-in problems — sign-in loops, "you don't have access", named-user vs shared-device confusion, and Admin Console assignment/entitlement gaps — from the Admin Console entitlement and the exact sign-in error, never by reinstalling first.
category: Troubleshooting Playbooks
tools: [search_tickets, search_knowledge_base, search_itglue, search_hudu, add_ticket_note, web_search]
---

# Adobe Creative Cloud Licensing

Most Creative Cloud "it won't open" tickets are licensing, not the app. The truth lives in the Adobe Admin Console: whether the user has the right identity type, is assigned the product, and whether this machine should be named-user or shared-device licensed. This playbook checks entitlement first and reinstalls last.

## When to use

- Creative Cloud apps prompt to sign in repeatedly, or a sign-in loop that never completes
- "You don't have access to this app" / trial or "your subscription has expired" for a licensed user
- Confusion between named-user licensing and shared-device (lab/classroom) licensing
- A new user can't get an app, or a departed user's license needs reclaiming; SSO/federated-ID sign-in fails

## Steps

1. **Licensing model and identity first.** search_itglue / search_hudu / search_knowledge_base for the client's Adobe setup: the plan type (Creative Cloud for teams vs enterprise/VIP/ETLA), the identity model (Adobe ID vs Business/Enterprise ID vs Federated SSO), whether any devices are **shared-device licensed** (labs, shared workstations) vs **named-user**, and who administers the Admin Console. Named-user vs shared-device is the fork that decides everything — establish it before touching the client.
2. **History first.** search_tickets for this client + Adobe: a recent license reassignment, an SSO/directory change, a plan renewal/expiry, or a device re-image. A user who "suddenly" lost access often had a license reclaimed or a plan lapse.
3. **Check entitlement before theorizing.** In the Adobe Admin Console (guide the admin): is the user present, with the correct identity type, and **assigned the specific product/product profile**? Is the plan active with seats available (not over-allocated)? For the app itself, the exact sign-in error and whether Creative Cloud desktop signs in at all. Read the entitlement — don't reinstall on a hunch.
4. **Branch:**
   1. **Sign-in loop / repeated prompts** — auth never sticks: usually a mismatch between the identity the user enters and the identity type Adobe expects (personal Adobe ID vs the org's Federated/Business ID), a Federated SSO problem (the IdP isn't returning the user, or the domain isn't claimed/linked), or stale local credentials. For SSO, test the federation and confirm the user's domain is linked to the directory; pair with the identity/SSO playbooks. Clearing the local Adobe credential/OOBE state is a clean step, but confirm the identity mismatch first.
   2. **"No access" / expired for a licensed user** — the user isn't assigned the product (or was unassigned), the plan has no free seats, or they're signed in with the wrong identity. Fix by assigning the correct product profile to the correct identity in the Console — seat allocation is the client's licensing (and cost) decision, so confirm before consuming a seat.
   3. **Named-user vs shared-device mismatch** — a shared/lab machine was licensed named-user (so it demands each person sign in and burns seats) or a personal workstation got the shared-device package. The fix is deploying the correct package for the use case (shared-device license package for labs, named-user for personal machines) — this is a packaging/deployment decision, not an end-user fix.
   4. **Provisioning / reclamation** — a new hire needs access or a leaver's seat must be reclaimed: assign/unassign in the Console (pair with onboarding-and-access / employee-offboarding). Reclaiming a departed user's seat frees the license; confirm no shared assets are stranded first.
5. **Verify and note.** Success is the user opening the actual licensed app signed in with the correct identity, with the seat showing assigned in the Console. Plain-text note: licensing model, identity type, entitlement finding, branch, action or handoff, verification.

## Guardrails

- No remote execution — Admin Console, packaging, and client steps are guidance for the Adobe admin / a tech; use get_ninjaone_device_link to reach a workstation when the RMM integration is enabled.
- Seat assignment is the client's licensing spend — confirm with the account owner before consuming or reclaiming a seat; don't buy/allocate seats on your own authority.
- Adobe accounts and assets tie to people — refer to users by placeholder and keep identity details out of PSA notes.
- Reinstalling Creative Cloud is a last resort, not a first move — it rarely fixes a licensing/identity problem and costs the user hours; confirm entitlement first.
- SSO/federation issues belong at the identity layer — pair with the SSO/identity playbooks rather than toggling Adobe settings blindly.
- Do not invent Console menu paths, package types, or plan behaviours — web_search Adobe's current admin docs and cite (Adobe changes these often).
- Docs coverage varies per tenant — note what you couldn't check. Notes destined for a PSA sync: plain text, no markdown or emojis.
