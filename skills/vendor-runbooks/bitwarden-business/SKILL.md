---
name: Bitwarden Business
description: A Bitwarden (Teams/Enterprise) rollout or admin ticket arrived — organization/collection structure, group-based sharing, account recovery (admin reset / trusted-device), and offboarding. Covers self-hosted caveats. Verify against Bitwarden's current documentation.
category: Vendor Runbooks
tools: [search_tickets, search_contacts, search_itglue, search_hudu, search_knowledge_base, add_ticket_note, create_ticket, update_ticket]
---

# Bitwarden Business

Vendor specialization of password-manager-rollout for Bitwarden (Teams / Enterprise organizations). password-manager-rollout owns the deployment canon — vault architecture, sharing discipline, emergency access, migration and decommission; this skill maps it onto Bitwarden's model (organizations, collections, groups, account recovery, and the cloud-vs-self-hosted choice). Verify feature names against Bitwarden's current documentation.

## When to use

- A client is deploying Bitwarden Teams/Enterprise and needs a rollout/structure plan
- A Bitwarden admin task lands: collection/group setup, sharing, or account recovery
- An employee is offboarding and their Bitwarden organization access must be handled

## Steps

1. Scope and structure per password-manager-rollout, in Bitwarden terms: each user keeps a personal vault (separate from the organization); shared credentials live in the **organization** and are partitioned into **collections** by team/function, with access granted through **groups** (Enterprise) rather than per-user collection assignments that drift. Privileged/infrastructure items go in a tightly-scoped collection. Record the structure in the client's documentation (search_itglue / search_hudu).
2. Group/collection discipline: manage access via groups → collections; the MSP's own scoped access (if any) is a defined group with least-privilege collection membership, documented. Note the boundary: personal vault items are the user's own and not org-visible — credentials that must survive a departure belong in an organization collection, not a personal vault.
3. Sharing discipline, written into the standard: share by collection/group membership, never by pasting a password into email/chat/tickets (Bitwarden **Send** is for one-off time-limited/expiring secret sharing with externals, not a substitute for collection sharing); one canonical item per credential; shared-account passwords rotate when a member leaves — wire into employee-offboarding.
4. Account recovery — the Bitwarden specifics:
   - Enterprise organizations support **account recovery / admin password reset** (an admin resets a member's master password) — this must be **enabled and the member enrolled** for it to work; enrollment is per-user, so confirm it at rollout, not at the crisis.
   - Where **trusted-device / SSO** login is used, understand the recovery implications (admin reset, or another trusted device) before relying on it.
   - Record WHO may invoke recovery and how it's logged; store any organization recovery material offline/sealed per the client's practice — never in a ticket or the doc platform in plaintext. Test the recovery path once before go-live.
5. Self-hosted caveat: if the client runs self-hosted Bitwarden, the MSP owns availability, backups, and updates of the server itself — the recovery path depends on that infrastructure being healthy. Flag self-hosted server backup/patching as its own responsibility (and its own monitoring), distinct from vault content.
6. Offboarding: remove the departing user from the organization (revoking collection access) and confirm their personal vault held no business credentials that should have been in a collection; flag every collection credential they could see for rotation. Migration and decommission per password-manager-rollout — inventory spreadsheets/browser stores (flag location, never reproduce contents), migrate privileged → shared → personal, rotate-flag every migrated credential (privileged first), delete old stores with evidence. Vault Health / breach reports (Enterprise) feed rotation priority. Track adoption like security-awareness-coordination; create tickets per phase.

## Guardrails

- Credentials never appear in tickets, notes, chat, or this skill's output — locations and counts only; spreadsheets found are flagged and ticketed, contents copied only into Bitwarden by the technician.
- Account recovery / admin reset only works if enabled AND the member is enrolled — confirm enrollment at rollout, or recovery won't exist when it's needed.
- Bitwarden Send is for external one-off shares, not internal credential sharing — sharing is collection/group membership.
- Self-hosted deployments make the MSP responsible for server backup/patching/availability — flag it as its own monitored responsibility.
- Organization recovery material stored offline/sealed per the client's practice — never in the PSA or doc platform in plaintext.
- Prefer moving business credentials into a collection before deprovisioning; rotate every departing-user-exposed shared credential, privileged first.
- Structure and recovery decisions get client sign-off; the agent plans and tracks, the technician and client admins execute in the console.
- Degradation: without documentation-tool access, the credential-store inventory relies on client interviews — say so, and expect it to be incomplete on the first pass.
