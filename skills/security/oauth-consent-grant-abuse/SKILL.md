---
name: OAuth Consent Grant Abuse
description: A malicious or over-privileged third-party app has an OAuth grant into a client's tenant (illicit consent / consent phishing) — identify the grant, revoke it, and tighten tenant consent policy so it can't recur.
category: Security
tools: [search_tickets, search_contacts, add_ticket_note, update_ticket, view_openDraft]
---

# OAuth Consent Grant Abuse

Consent phishing skips the password entirely: the user is tricked into granting a rogue app OAuth permissions (read mail, send mail, read files), and the app now holds durable access via a refresh token — no password to reset, MFA never challenged again. This runbook finds the grant, revokes it, and closes the consent path.

## When to use

- An alert fires for a newly consented enterprise app, a risky OAuth grant, or an unfamiliar app with mail/file permissions
- A user reports approving an app "to view a document" or "to install a plugin" that then behaved oddly
- Persistence hunting during account-takeover-runbook / business-email-compromise-recovery surfaces an attacker-added app consent

## Steps

1. Identify the grant: which app, which account(s) consented, what permissions (delegated vs. application/tenant-wide), and when. Application-level and admin-consented grants are the most dangerous — they can persist beyond any single user.
2. Judge legitimacy before revoking blindly: is this a known business app the client actually uses, or an unfamiliar/lookalike app requesting broad mail/file scopes? Broad "read/send mail" or "read all files" scopes on an unknown app is malicious until proven otherwise. Check publisher verification and consent timestamp against any sign-in anomaly.
3. Revoke on the malicious verdict: direct the client's admin to remove the app's grant/service-principal and revoke its issued tokens tenant-side — this kills the refresh token that gives the app standing access. Password resets do NOT touch an OAuth grant; the app keeps access until the grant itself is revoked. The agent drives and documents; the admin executes.
4. Assess what the app could reach during its access window: mailbox contents, files, directory data. If mail was accessible and fraud may have flowed, branch to business-email-compromise-recovery. Preserve the grant definition and app audit logs before removal.
5. Close the vector tenant-wide: recommend the client restrict user consent (require admin approval for third-party apps, or limit consent to verified publishers and low-risk scopes) and enable an admin-consent request workflow. This is the fix that stops the next consent-phish — flag it as the priority remediation, owned by the client's admin.
6. Check for co-planted persistence (inbox rules, forwarding, delegates) if the grant accompanied a takeover.
7. Document the decision, not just the action: the app, its scopes, the legitimacy reasoning, what was revoked and when, the access-window assessment, and the consent-policy recommendation with its owner. Classify per soc-classification-tree.

## Guardrails

- An OAuth grant is separate persistence — a password reset and session revocation do NOT remove it. Revoke the grant explicitly or the app stays in.
- Revoking grants and changing tenant consent policy are admin actions owned by the client — recommend and drive; the agent does not modify tenant app registrations or consent settings itself, and tightening consent policy is an explicit client decision.
- Don't revoke a legitimate business app on reflex — verify the app and scopes first; a wrongful revoke can break a real integration. But broad scopes on an unknown/unverified publisher outvote convenience.
- Tenant-wide (admin-consented / application) grants outrank single-user grants in urgency — they survive individual account cleanup.
- Defensive writing: "an unauthorized application grant was detected and removed," not "your tenant was breached"; do not assert data exfiltration without log evidence.
- Plain text only for PSA-synced notes.
- When in doubt, escalate and restrict consent — a briefly interrupted app is recoverable; a standing rogue grant reading mail is not.
