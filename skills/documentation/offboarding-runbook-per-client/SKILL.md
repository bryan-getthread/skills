---
name: Offboarding Runbook Per Client
description: Document the repeatable leaver runbook for a specific client's stack — the exact accounts to disable, access to revoke, licenses to reclaim, data to preserve, and hardware to recover — so every offboarding at that client is complete and consistent.
category: Documentation
tools: [search_clients, search_tickets, search_knowledge_base, search_itglue, search_hudu, notion-search, notion-create-pages, notion-update-page, add_ticket_note]
---

# Offboarding Runbook Per Client

Produces the standing, client-specific leaver runbook: for this client, exactly what has to happen when someone departs — accounts disabled, access revoked, sessions killed, licenses reclaimed, mail and data preserved and reassigned, and hardware recovered — captured as a repeatable checklist against their real stack. It is the reference the desk follows every time this client offboards someone, so nothing gets missed. Executing a specific offboarding is onboarding-and-access/employee-offboarding; auditing completeness is onboarding-and-access/offboarding-completeness-audit. This skill documents the client's repeatable template.

## When to use

- "Document the standard leaver process for <client> so we never miss a step."
- A client offboards staff regularly and departures are handled inconsistently.
- Onboarding a client and building their runbooks in the doc pack (see documentation/client-onboarding-documentation-pack).
- A security or audit review found offboarding gaps (lingering access, unreclaimed licenses) that a documented runbook would prevent.

## Steps

1. Establish the client with `search_clients`, then reconstruct the real pattern from evidence: `search_tickets` for past offboarding/leaver tickets at this client (what was disabled/revoked/recovered, and anything that was missed and caused a follow-up); `search_itglue` / `search_hudu` (where enabled), `search_knowledge_base`, and `notion-search` (if connected) for existing offboarding docs and the client's environment/access records.
2. Build the **per-client offboarding runbook**, grounded in that evidence and ordered so access is cut promptly:
   - **Access termination (do first)** — disable the directory account, revoke MFA/tokens, and force sign-out of active sessions across the client's identity platform. Note that this is the time-critical step.
   - **Group & permission removal** — remove from security/distribution groups, shared mailboxes, and delegated permissions.
   - **Mail & data** — set auto-reply/forwarding per the client's policy, convert or delegate the mailbox, and preserve/reassign the leaver's files and data owner-by-owner. Include litigation-hold handling where the client requires it (see onboarding-and-access/litigation-hold).
   - **Licenses & subscriptions** — reclaim and reassign licenses (feed documentation/warranty-license-registry).
   - **Credentials** — rotate any shared or privileged credentials the leaver knew (see documentation/password-rotation-documentation).
   - **Hardware** — recover and wipe/re-image devices; peripheral and access-badge return (see onboarding-and-access/laptop-return-logistics).
   - **Third-party & LOB apps** — deprovision the client's line-of-business and vendor apps that aren't governed by the directory.
   - **Verification & sign-off** — the checks that confirm access is fully gone, plus who signs off.
3. Flag gaps: steps missed in past offboardings, access that lives outside the directory and is easy to overlook, and any policy question (retention, forwarding) that only the client can answer. These are decisions to confirm.
4. Set the **storage and cadence discipline**: the runbook lives under the client's section in the desk's documentation platform, dated, reviewed when the client's stack changes. Confirm the location convention rather than inventing one.
5. Output the runbook as a paste-ready, ordered checklist plus the gap/decision list. If the desk keeps runbooks in Notion and the destination is confirmed, create with `notion-create-pages` or refresh with `notion-update-page`; otherwise paste-ready blocks. When a specific departure needs doing, an offboarding ticket can be raised from the runbook via `add_ticket_note` (execution belongs to onboarding-and-access/employee-offboarding).

## Guardrails

- **Documents the repeatable template, from real evidence — it does not deprovision anyone.** Every step traces to what past offboardings at this client did or to a client-confirmed policy; do not invent access to revoke or assume where data lives.
- **Access-termination-first ordering** is deliberate — the runbook must front-load cutting access; delays are a security exposure.
- **No credentials in the runbook**; shared-credential rotation is referenced by process, and secrets stay in the vault (see documentation/credential-storage-audit).
- Retention, forwarding, and hold decisions are the client's to make — flag them for confirmation, never default them silently.
- Notes and checklists are plain text per the Note Format Standard (documentation/note-format-standard) — they may sync to a PSA.
- Docs tools are search-only; writes go to Notion only when the member has connected it and confirmed the destination, otherwise paste-ready output. Degradation: without IT Glue/Hudu, build from offboarding ticket history and requester knowledge and mark the data/license/app sections as needing client confirmation.
