---
name: 1Password Business
description: A 1Password Business rollout or admin ticket arrived — vault and group structure, sharing discipline, the Emergency Kit and admin/recovery-group account recovery, and offboarding via suspend-then-recover. Verify against 1Password's current documentation.
category: Vendor Runbooks
tools: [search_tickets, search_contacts, search_itglue, search_hudu, search_knowledge_base, add_ticket_note, create_ticket, update_ticket]
---

# 1Password Business

Vendor specialization of password-manager-rollout for 1Password Business. password-manager-rollout owns the deployment canon — vault architecture, sharing discipline, emergency access, migration and decommission; this skill maps it onto 1Password's model (Private vaults, shared vaults, groups, the Emergency Kit and Secret Key, and administrator-initiated account recovery). Verify feature names against 1Password's current documentation.

## When to use

- A client is deploying 1Password Business and needs a rollout/structure plan
- A 1Password admin task lands: vault/group setup, sharing, or account recovery
- An employee is offboarding and their 1Password account must be suspended/recovered

## Steps

1. Scope and structure per password-manager-rollout, in 1Password terms: every user has a **Private vault** (invisible to admins by design — admins cannot see its contents); shared **vaults** by team/function granted via **groups**, never one giant company vault — vault access is managed by group membership. Privileged/infrastructure credentials go in a tightly-scoped admin vault. Record the structure in the client's documentation (search_itglue / search_hudu).
2. Groups and access discipline: manage vault access through groups, not per-person grants that drift; the MSP's own scoped access (if any) is a defined group with least-privilege vault membership, documented. Note the design consequence up front — because Private vaults are invisible to admins, credentials that must survive a departure belong in a shared vault, not a Private one; make that a usage rule.
3. Sharing discipline, written into the standard: share by vault/group membership, never by pasting a password into email/chat/tickets (use item-sharing links only where the client's policy allows, with expiry); one canonical item per credential; shared-account passwords rotate when a vault member leaves — wire into employee-offboarding.
4. Emergency access & recovery — the 1Password specifics:
   - Every user's **Emergency Kit** (containing the Secret Key) must be stored per the client's documented secure-storage practice (printed/sealed offline — never in a ticket, never in the system it recovers). The Secret Key cannot be reset; losing it without recovery access can lock the account out.
   - Business accounts support **administrator-initiated account recovery** (an admin or the Recovery Group re-provisions a user who lost their master password/Secret Key). Confirm the Recovery Group membership is defined and least-privilege at rollout, record WHO may invoke recovery and how it's logged, and test the recovery path once before go-live.
5. Offboarding — suspend, then recover if needed: at departure, **suspend** the user (preserves the account and its vault access record without deleting) rather than immediately deleting; if the client needs the departing user's items, use admin account recovery to gain access, move any business-critical items into a shared vault, then deprovision. Flag every shared credential the user could see for rotation.
6. Migration and decommission per password-manager-rollout: inventory credential spreadsheets/browser stores (flag location, never reproduce contents), migrate privileged → shared → personal, rotate-flag every migrated credential (privileged first), delete old stores with evidence. Watchtower findings feed rotation priority. Track adoption like security-awareness-coordination; create tickets per phase.

## Guardrails

- Credentials never appear in tickets, notes, chat, or this skill's output — locations and counts only; spreadsheets found are flagged and ticketed, contents copied only into 1Password by the technician.
- Private vaults are invisible to admins by design — credentials that must survive offboarding belong in a shared vault; make this a rule, not a surprise.
- The Emergency Kit / Secret Key is stored offline/sealed per the client's practice — never in the PSA, never in the doc platform in plaintext; the Secret Key cannot be reset.
- Confirm the Recovery Group is defined and least-privilege before go-live; recovery invocations are logged and named.
- Prefer suspend over delete at offboarding so recovery stays possible; rotate every departing-user-exposed shared credential, privileged first.
- Vault/group and recovery decisions get client sign-off; the agent plans and tracks, the technician and client admins execute in the console.
- Degradation: without documentation-tool access, the credential-store inventory relies on client interviews — say so, and expect it to be incomplete on the first pass.
