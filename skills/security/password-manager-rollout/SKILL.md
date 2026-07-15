---
name: Password Manager Rollout
description: A client is deploying a password manager — plan vault structure, sharing discipline, emergency access, and the migration off spreadsheets and browser stores, flagging every credential spreadsheet found along the way.
category: Security
tools: [search_tickets, search_contacts, search_itglue, search_hudu, search_knowledge_base, add_ticket_note, create_ticket, update_ticket]
---

# Password Manager Rollout

The deployment plan that decides whether a password manager becomes the client's credential system of record or an abandoned browser extension: vault architecture, sharing rules people can actually follow, break-glass access that works when the admin is unreachable, and a migration that hunts down every credential spreadsheet.

## When to use

- A client purchased (or is choosing) a password manager and needs a rollout plan
- Credential spreadsheets or shared-password chaos surfaced and a manager rollout is the fix
- A stalled deployment needs an adoption push and cleanup of the old credential stores

## Steps

1. Scope the deployment: user count and groups (search_contacts), whether the MSP's own technicians need scoped access to client vaults, SSO/directory integration for provisioning, and the client's compliance drivers. Confirm platform choice is made — this skill deploys, it doesn't sell; comparative selection belongs with the account manager/vCIO.
2. Design the vault structure before inviting anyone:
   - Personal vaults: every user, for their own credentials — private by default.
   - Shared vaults/collections by team or function (finance, operations, IT), never one giant "Company" vault — sharing scope should mirror who actually needs each credential.
   - Admin/privileged vault: tightly held, for infrastructure credentials.
   Record the structure in the client's documentation (search_itglue / search_hudu for where their IT docs live).
3. Sharing discipline, written into the client's usage standard: credentials are shared by vault membership, never by pasting a password into email/chat/tickets; one credential lives in one canonical entry (no per-user copies that drift); shared-account passwords rotate when a vault member leaves — wire that into the employee-offboarding checklist.
4. Emergency access — decided now, not during the emergency: designate break-glass access per the platform's mechanism (emergency-access contacts, admin recovery kit, or sealed recovery codes), store the recovery material per the client's documented secure-storage practice (offline/sealed — never in a ticket, never in the same system it recovers), and record WHO may invoke it and how the invocation is logged. Test the recovery path once before go-live.
5. Migration from spreadsheets and browser stores: inventory current credential locations — ask the client, and search_itglue / search_hudu / search_tickets for signs of credential files ("passwords.xlsx" and kin). Sanitization canon applies to every one found: flag its existence and location in a plain-text note WITHOUT reproducing its contents — never paste credentials into a ticket, ever. Migration order: privileged/infrastructure credentials first, then shared team credentials, then personal imports (browser-store import tools). Every migrated credential marked for rotation — a password that lived in a spreadsheet is a compromised-until-rotated password, prioritized by privilege.
6. Decommission the old stores — the step that always gets skipped: after verified migration, the spreadsheets are deleted (not "kept just in case"), browser password saving is disabled by policy where the client's management agrees, and the deletion is recorded. An un-decommissioned spreadsheet quietly becomes the live store again within a quarter.
7. Rollout mechanics: pilot group first, then phased enrollment with a short how-to note per the client's communication standard; track enrollment/adoption like the security-awareness-coordination skill tracks training (respectful nudges, program-owner escalation). Create tickets (create_ticket) for each phase; report adoption and rotation progress to the client.

## Guardrails

- Credentials never appear in tickets, notes, chat, or this skill's output — locations and counts only. Any credential spreadsheet found is flagged and its migration ticketed; its contents are never copied anywhere except directly into the vault by the technician.
- Emergency-access material is stored offline/sealed per the client's practice — never in the PSA, never in the documentation platform in plaintext, never in the vault it recovers.
- Migrated credentials are rotate-flagged by default, privileged ones first; migration without rotation just changes where the exposed password sits.
- Old stores are decommissioned with evidence, or the rollout is not done.
- Vault-structure and policy decisions get client sign-off; the agent plans and tracks, the technician and client admins execute in the platform.
- Degradation: without documentation-tool access, the credential-store inventory relies on client interviews — say so, and expect the inventory to be incomplete on the first pass.
