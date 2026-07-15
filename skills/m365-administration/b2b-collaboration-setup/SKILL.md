---
name: B2B Collaboration Setup
description: Set up cross-tenant collaboration between a client and a partner organization — scoped cross-tenant access settings, MFA/device trust decisions, and a documented rollback. Use when a ticket asks to "let <partner org>'s users into our tenant", fix double-MFA for external users, or restrict which domains can be invited.
category: M365 Administration
tools: [search_tickets, search_knowledge_base, search_itglue, search_hudu, add_ticket_note, update_ticket, send_approval, web_search]
---

# B2B Collaboration Setup

Cross-tenant access settings are tenant-to-tenant trust decisions dressed up
as collaboration convenience. This skill scopes the trust to the named
partner, decides the MFA-trust question deliberately, and writes the rollback
before anything is changed.

## When to use

- "<client> is partnering with <organization> — their people need access to
  our Teams/SharePoint."
- External users complain about MFA twice (home tenant and again as guest).
- "Restrict guest invites to approved partner domains only."
- Reviewing existing cross-tenant settings during onboarding or an audit.

## Steps

1. **Intake — scope the actual need.** Which partner organization (their
   tenant domain), which direction (their users into the client's tenant =
   inbound; client's users into theirs = outbound), which of the client's
   users/groups and apps are in scope, and for how long (project-bound or
   ongoing). "Everyone, everything, forever" is a red flag to push back on,
   not a requirement to implement.
2. **Docs and current state.** search_itglue / search_hudu /
   search_knowledge_base for the client's external-collaboration standard.
   Tech reads the current cross-tenant access defaults and any existing
   org-specific entries — a partner may already be configured, and the fix is
   adjusting scope, not adding a duplicate.
3. **Prefer an org-specific entry over loosening defaults.** Add the partner
   tenant as a named organization in cross-tenant access settings and scope
   inbound access to the specific users/groups and applications requested.
   Tenant-wide default changes affect every current and future external org —
   they need their own justification and approval, separate from this request.
4. **Decide the trust claims deliberately.** The double-MFA fix is trusting
   MFA claims from the partner's home tenant. That is a real trust decision:
   the client is accepting the partner's MFA quality as their own. Reasonable
   for a known, managed partner; state it plainly in the approval. Trusting
   compliant-device or hybrid-join claims is a bigger step — only when the
   partner's device management is known and the client's CA policies depend
   on it; default to NOT trusting device claims.
5. **Pair with invite hygiene.** If the client wants domain restriction, set
   the collaboration allow/deny list to the approved partner domains — an
   allowlist is the durable posture; a denylist is whack-a-mole. Confirm this
   doesn't strand existing guests from other domains (cross-reference the
   guest-access-audit skill for who is already inside).
6. **Approval gate.** send_approval to the client's documented authority
   with: partner tenant, direction, scoped users/groups/apps, trust claims
   accepted (MFA yes/no, device claims yes/no), duration/review date, and the
   rollback — remove or restrict the org-specific entry, which cuts the
   partner's access cleanly without touching individual guests.
7. **Verify and document.** Test with one partner user: access works, and the
   MFA experience matches the decision made. Plain-text note: partner org,
   direction, scope, trust decisions, approver, review date, rollback steps.
   Store the full configuration record in IT Glue/Hudu.

## Guardrails

- Never change tenant-wide cross-tenant defaults to satisfy a single-partner
  request — org-specific settings exist for exactly this.
- MFA-trust and device-trust decisions are named explicitly in the approval;
  they must never ride along silently in "set up the partner access".
- Ongoing collaborations get a review date; project-bound ones get an end
  date and a tracked removal — trust without expiry is the failure mode this
  skill exists to prevent.
- Do not trust device-compliance claims from a partner tenant by default;
  that imports their device posture into the client's access decisions.
- Agent prepares scope, trust decisions, and rollback; the technician
  executes in Entra. Verify against Microsoft's current cross-tenant docs
  (web_search) — this settings surface changes frequently.
