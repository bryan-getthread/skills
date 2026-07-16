---
name: Onboarding Runbook Per Client
description: Document the repeatable new-hire onboarding runbook for a specific client's stack — the exact accounts, groups, licenses, hardware, and app access a new starter at that client gets — so every onboarding at that client is provisioned the same way.
category: Documentation
tools: [search_clients, search_tickets, search_knowledge_base, search_itglue, search_hudu, notion-search, notion-create-pages, notion-update-page, add_ticket_note]
---

# Onboarding Runbook Per Client

Produces the standing, client-specific new-hire runbook: for this client, exactly what a new starter needs provisioned — accounts, group memberships, licenses, hardware, and application access — captured as a repeatable checklist tied to their real stack. It is the reference the desk follows every time this client onboards someone, so onboardings stop being reconstructed from memory. Executing a specific onboarding is onboarding-and-access/new-hire-onboarding; this skill documents the client's repeatable template.

## When to use

- "Document the standard new-hire setup for <client> so we do it the same way every time."
- A client onboards staff regularly and each one is provisioned slightly differently.
- Onboarding a client and building their runbooks in the doc pack (see documentation/client-onboarding-documentation-pack).
- A role-based variation is needed (e.g. the client's warehouse staff vs. office staff get different access) and the differences should be written down.

## Steps

1. Establish the client with `search_clients`, then reconstruct the real pattern from evidence: `search_tickets` for past new-hire/onboarding tickets at this client (what was actually provisioned, in what order, by whom); `search_itglue` / `search_hudu` (where enabled), `search_knowledge_base`, and `notion-search` (if connected) for existing onboarding docs and the client's environment/access records.
2. Build the **per-client onboarding runbook**, grounded in that evidence:
   - **Identity & accounts** — the client's directory (M365/Google/AD), account naming convention, and how the account is created; MFA enrollment.
   - **Group & access memberships** — the security/distribution groups, shared mailboxes, and permission sets a standard starter gets (see onboarding-and-access/distribution-list-management, shared-mailbox-delegation for the mechanics).
   - **Licenses** — which licenses/subscriptions are assigned by default (cross-ref documentation/warranty-license-registry for availability).
   - **Hardware** — the standard device build, imaging/enrollment process, and peripherals.
   - **Applications** — the client's line-of-business apps and how access to each is granted (who approves, which portal).
   - **Role variations** — where different roles/departments at this client get different provisioning, documented as named variants rather than one-size-fits-all.
   - **Handover & verification** — welcome/orientation steps (onboarding-and-access/orientation-prep), and the checks that confirm the starter can actually log in and work.
3. Flag gaps: steps that appear in some past onboardings but not others (inconsistency to resolve with the client), and any access whose approval path is unclear. These are decisions to confirm, not blanks to fill.
4. Set the **storage and cadence discipline**: the runbook lives under the client's section in the desk's documentation platform, dated, reviewed when the client's stack changes. Confirm the location convention rather than inventing one.
5. Output the runbook as a paste-ready checklist (with role variants) plus the gap/decision list. If the desk keeps runbooks in Notion and the destination is confirmed, create with `notion-create-pages` or refresh with `notion-update-page`; otherwise paste-ready blocks. When a specific hire needs doing, an onboarding ticket can be raised from the runbook via `add_ticket_note` on the intake ticket (execution belongs to onboarding-and-access/new-hire-onboarding).

## Guardrails

- **Documents the repeatable template, from real evidence — it does not provision anyone.** Every step traces to what past onboardings at this client actually did or to a client-confirmed standard; do not invent access grants or assume a group exists.
- **No credentials in the runbook** — temporary passwords and secrets are handled at execution time and vaulted; the runbook references the vault and the process, not values (see documentation/credential-storage-audit and onboarding-and-access/password-and-mfa-recovery).
- Least-privilege posture: the default access set is the baseline a standard starter needs — extra access is a separate request, not baked into the template silently.
- Notes and checklists are plain text per the Note Format Standard (documentation/note-format-standard) — they may sync to a PSA.
- Docs tools are search-only; writes go to Notion only when the member has connected it and confirmed the destination, otherwise paste-ready output. Degradation: without IT Glue/Hudu, build from onboarding ticket history and requester knowledge and mark the access/license sections as needing client confirmation.
