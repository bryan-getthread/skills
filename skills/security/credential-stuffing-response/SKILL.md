---
name: Credential Stuffing Response
description: A pattern of failed/anomalous logins across many accounts points to password spraying or credential stuffing — scope the attack, lock down, and rotate the accounts that actually fell.
category: Security
tools: [search_tickets, search_contacts, add_ticket_note, update_ticket, view_openDraft]
---

# Credential Stuffing Response

The signature is breadth, not depth: many accounts hit, often one or few password attempts each (spraying), or a flood of logins using credentials leaked elsewhere (stuffing). This runbook scopes the campaign, hardens the front door, and rotates the accounts that actually succumbed — without drowning the desk in per-account tickets.

## When to use

- A spike of failed sign-ins spread across many accounts, or repeated logins from unfamiliar IPs/ranges
- An "unusual sign-in volume," "password spray," or "credential stuffing" alert
- A known third-party breach dump prompts a check of whether reused passwords are being replayed against the client's tenant

## Steps

1. Scope the campaign before touching individual accounts: how many accounts targeted, over what window, from which IPs/ranges/geographies, and — the pivotal split — which attempts FAILED versus which SUCCEEDED. A thousand failures is noise-to-harden; one success is a takeover to contain.
2. For any account where a login SUCCEEDED: treat as compromised — contain per account-takeover-runbook (rotate password, revoke sessions/tokens, sweep rules/consents). Successful stuffing means that password was valid and is now known.
3. For the targeted-but-failed population: the passwords weren't guessed, but the accounts are now known targets. Prioritize MFA coverage on any that lack it and flag legacy-auth / non-MFA sign-in paths, which spraying specifically hunts for.
4. Harden the front door at the tenant level (client admin-owned, recommend and drive): enforce MFA on all accounts, disable legacy/basic authentication, enable smart lockout / anomalous-sign-in protection, and consider conditional-access blocks on the attacking IP ranges/geographies. This is the fix that turns the campaign into noise.
5. Address exposed/reused credentials: if the source is a known breach dump of reused passwords, drive rotation for the affected users (branch to breached-credential-response) and reinforce that reused passwords are the fuel — password-manager-rollout is the durable fix.
6. Correlate, don't spam: manage the campaign as one incident with a scoped account list, not one ticket per failed login. search_tickets to fold in related alerts.
7. Document the decision, not just the action: attack scope (accounts, IPs, window), the success/fail split, which accounts were contained, the hardening recommendations with their owner, and residual risk. Classify per soc-classification-tree.

## Guardrails

- Separate failures from successes on the first pass — the response diverges completely. Contain the successes; harden around the failures. Don't reset a thousand accounts that never fell, and don't leave the one that did.
- Lockout policy, MFA enforcement, legacy-auth disablement, and conditional-access changes are tenant-wide admin actions owned by the client — recommend and drive; the agent does not flip tenant auth settings itself, and these are explicit client decisions.
- Beware over-aggressive lockout as a self-inflicted denial of service — a spray can be designed to lock out real users; tune lockout to block attackers without freezing the workforce, and coordinate with the client.
- A successful stuffing login = compromised account = full containment; never soft-close it as "just one of many attempts."
- Defensive writing: "a login attack targeting multiple accounts was detected," not "you were breached"; only accounts with confirmed successful sign-in are described as compromised.
- Plain text only for PSA-synced notes.
- When in doubt, harden MFA and contain the confirmed successes — broad hardening is low-cost; a missed successful login is a live takeover.
