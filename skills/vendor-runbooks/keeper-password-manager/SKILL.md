---
name: Keeper Password Manager
description: A Keeper Security rollout or admin ticket arrived — vault/record and shared-folder structure, role-enforced sharing discipline, break-glass access, and offboarding vault transfer via Account Transfer. Verify against Keeper's current documentation.
category: Vendor Runbooks
tools: [search_tickets, search_contacts, search_itglue, search_hudu, search_knowledge_base, add_ticket_note, create_ticket, update_ticket]
---

# Keeper Password Manager

Vendor specialization of password-manager-rollout for Keeper Security. password-manager-rollout owns the deployment canon — vault architecture, sharing discipline, emergency access, migration and decommission; this skill maps that canon onto Keeper's model (records, folders and shared folders, the Admin Console with roles and enforcement policies, and Account Transfer for offboarding). Verify feature names against Keeper's current documentation.

## When to use

- A client is deploying Keeper and needs a rollout/structure plan
- A Keeper admin task lands: sharing setup, role/enforcement policy, or emergency access
- An employee is offboarding and their Keeper vault must be transferred/retained

## Steps

1. Scope and structure per password-manager-rollout, in Keeper terms: personal vaults private by default; **shared folders** by team/function (finance, ops, IT) rather than one giant company folder — Keeper permissions are set per shared folder and per record, so scope mirrors who needs each credential. The privileged/infrastructure records go in a tightly-held shared folder. Record the structure in the client's documentation (search_itglue / search_hudu).
2. Role and enforcement discipline via the Keeper Admin Console: roles carry **enforcement policies** (master-password/2FA requirements, sharing restrictions, export controls, platform restrictions). Set these as the client's usage standard before mass enrollment — enforcement applied after the fact is a migration in itself. If the MSP's own technicians need scoped access to client vaults, that is a defined role with its own shared-folder membership, documented and least-privilege.
3. Sharing discipline, written into the standard: credentials are shared by shared-folder/record permission, never by pasting a password into email/chat/tickets; one canonical record per credential (no per-user copies that drift); shared-account passwords rotate when a folder member leaves — wire that into employee-offboarding.
4. Emergency / break-glass access — decided now, not during the emergency: designate Keeper's emergency-access mechanism, store the recovery material per the client's documented secure-storage practice (offline/sealed — never in a ticket, never in the system it recovers), record WHO may invoke it and how the invocation is logged, and test the path once before go-live.
5. Offboarding vault transfer — the Keeper-specific step: Keeper's **Account Transfer** policy (enabled per role BEFORE it's needed — it cannot be applied retroactively to an existing account) lets an admin transfer a departing user's vault to another user. Verify the transfer policy is in place as part of rollout, and at offboarding: confirm authorization, transfer the vault to the named receiving user, then deprovision — and flag every shared credential the departing user could see for rotation.
6. Migration and decommission per password-manager-rollout: inventory credential spreadsheets/browser stores (flag existence and location, never reproduce contents), migrate privileged → shared → personal, rotate-flag every migrated credential (privileged first), then delete the old stores with evidence. BreachWatch findings (if the client licenses it) feed rotation priority. Track enrollment/adoption like security-awareness-coordination tracks training; create tickets per phase.

## Guardrails

- Credentials never appear in tickets, notes, chat, or this skill's output — locations and counts only; any credential spreadsheet found is flagged and ticketed, contents copied only into the vault by the technician.
- Account Transfer must be enabled by role BEFORE a user leaves — it cannot be applied to an existing account retroactively; verify this at rollout, not at offboarding.
- Emergency-access/recovery material is stored offline/sealed per the client's practice — never in the PSA, never in the doc platform in plaintext, never in the vault it recovers.
- Migrated and offboarding-exposed credentials are rotate-flagged by default, privileged first.
- Old stores are decommissioned with evidence, or the rollout is not done.
- Vault-structure, role, and enforcement decisions get client sign-off; the agent plans and tracks, the technician and client admins execute in the Admin Console.
- Degradation: without documentation-tool access, the credential-store inventory relies on client interviews — say so, and expect it to be incomplete on the first pass.
