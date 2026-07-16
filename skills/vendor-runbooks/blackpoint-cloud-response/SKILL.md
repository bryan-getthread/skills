---
name: Blackpoint Cloud Response
description: A Blackpoint Cloud Response (M365/identity) alert landed — their SOC often disabled the account or revoked sessions already; confirm the cloud-side actions, run the full identity persistence sweep they don't, and merge the companion ticket storm.
category: Vendor Runbooks
tools: [search_tickets, search_clients, search_contacts, merge_ticket, add_ticket_note, update_ticket]
---

# Blackpoint Cloud Response

Specialization of blackpoint-soc-response for Blackpoint's Cloud Response — their MDR coverage of Microsoft 365 / cloud identity, as opposed to the endpoint/network response the parent skill centers on. The parent skill owns the SOC-already-contained posture, phone-call intake, storm-merge, and coordinate-restoration mechanics; this skill adds what's specific to a *cloud/identity* Blackpoint response: the actions their SOC takes are identity-plane (disable account, revoke/kill sessions, block sign-in), and the follow-through is a full identity persistence sweep, because disabling an account does not evict an attacker's other footholds. Verify Cloud Response scope against Blackpoint's current documentation and the client's service agreement. Read blackpoint-soc-response first; do not duplicate its steps.

## When to use

- A Blackpoint Cloud Response alert/incident about an M365 or cloud-identity event lands (suspicious sign-in, token/session abuse, mailbox-rule or OAuth abuse, admin change) where their SOC already acted
- A Blackpoint analyst reported disabling a cloud account or revoking sessions and the identity follow-up is owed
- Cloud-identity Blackpoint tickets need consolidating with their endpoint/RMM siblings

## Steps

1. Apply blackpoint-soc-response's intake and "actions taken" parse first — classify every SOC-listed action as contained-already and everything else as needs-action. Here the SOC actions are identity-plane: account disabled, sessions revoked/killed, sign-in blocked.
2. Verify the cloud-side containment by effect (technician confirms in the identity provider / M365 admin): the account is actually disabled, sessions are actually revoked, sign-in is actually blocked. A claimed session-revoke that left a live refresh token is exactly the failure mode here.
3. Run the full identity persistence sweep — the core of this skill — per compromised-account-containment, because Blackpoint's disable/revoke covers sign-in, not persistence. Sweep: password reset, MFA methods (attacker-registered authenticators), app passwords / legacy-auth, OAuth app grants and consents, mailbox rules and forwarding, delegated/mailbox permissions, and any newly created accounts. Each is a foothold that survives an account disable.
4. Scope across the tenant and to endpoints: search prior/concurrent context (search_tickets, same tenant/user/indicator, recent window) and branch to edr-detection-runbook if a device is implicated — cloud takeover and endpoint compromise often accompany each other.
5. Storm-merge companion tickets per blackpoint-soc-response (the cloud alert, "I can't log in" from the disabled account, MFA-reset requests, monitoring noise) with exact references — never on wording alone.
6. Coordinate restoration with Blackpoint's SOC: re-enabling the account happens only after the sweep is complete and verified, and jointly with them — record who agreed and when.
7. Document the split timeline (Blackpoint cloud actions with their timestamps vs desk sweep with yours) and classify per soc-classification-tree. Client-facing wording per defensive-writing-standard.

## Guardrails

- A disabled account is not a contained attacker — the persistence sweep (MFA, app passwords, OAuth grants, mail rules, delegates) is the actual work here; never close on "Blackpoint disabled the user."
- Verify session revocation by effect — a surviving refresh token or app password reopens the account silently.
- Do not re-enable the account to quiet lockout complaints — restoration follows the completed sweep and coordination with Blackpoint's SOC.
- Verbal claims from a caller get verified, and confirm the caller is genuinely Blackpoint via the documented path — attackers impersonate SOCs.
- Merge with exact references only; keep siblings' evidence in the primary ticket.
- The agent has no Blackpoint portal or tenant-admin access — cloud-side verification, the sweep actions, and restoration are technician steps the agent directs and records.
- Do not duplicate blackpoint-soc-response — this skill is the cloud/identity follow-through layer on top of it.
