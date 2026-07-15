---
name: Liongard M365 Tenant Read
description: Answer tenant-level Microsoft 365 questions from the Liongard M365 inspector's dataprint — license inventory and assignment, mailbox statistics, admin roles, secure-score inputs, and sharing settings — without touching the tenant.
category: Liongard Inspectors
tools: [liongard_launchpoint, liongard_metric, liongard_identity, liongard_detection, liongard_timeline, liongard_query, search_tickets, add_ticket_note]
---

# Liongard M365 Tenant Read

Interrogate a client's Microsoft 365 tenant through Liongard's Microsoft 365 inspector: the last-captured dataprint holds users, licenses, mailboxes, admin roles, and tenant settings, so most "how many / who has / is it enabled" questions are answerable in seconds with zero tenant access. This is the read plane; execution (changing licenses, policies, mailboxes) belongs to the m365-administration skills.

## When to use

- "How many global admins are in <client>'s M365 tenant?" / "who holds admin roles there?"
- "How many <license SKU> licenses does <client> have, and how many are unassigned?"
- "Which mailboxes at <client> are near quota / oversized / have forwarding set?"
- "Is external sharing on for <client>?" / secure-score-input questions (MFA, legacy auth, sharing)
- Any M365 tenant fact needed for a ticket, quote, or QBR where reading is enough

## Steps

1. Follow the base access pattern (skills/liongard-inspectors/liongard-access-pattern/SKILL.md): liongard_launchpoint filtered by systemType Microsoft 365 for the client's environment. Confirm the launchpoint exists and the last run succeeded; capture the last-run timestamp — every answer carries "as of <timestamp>." A failed or ancient run means the answer is stale: say so and offer a technician-side refresh before anyone acts on it.
2. Route the question:
   - Identity-shaped (admins, MFA, stale accounts) → prefer liongard_identity for the reconciled view, then liongard_metric on the dataprint for tenant-specific detail. For deep admin work hand off to security/global-admin-audit and security/identity-mfa-health-check — this skill supplies their raw reads, not their judgment.
   - License-shaped → liongard_metric against the subscribed-SKU and per-user license-assignment sections: total vs. consumed per SKU, unassigned count, and licensed-but-disabled accounts (money leaking). Verify field paths against the live dataprint — example angles like `Skus[?SkuPartNumber=='SPE_E3'].{total: PrepaidUnits.Enabled, used: ConsumedUnits}` or `Users[?AccountEnabled==\`false\` && length(AssignedLicenses) > \`0\`]` are shapes, not gospel; dataprint schemas vary by inspector version.
   - Mailbox-shaped → metric angles over mailbox statistics: size vs. quota, item counts, forwarding addresses, litigation hold. A forwarding address pointing outside the client's domains is a security lead, not a stat — route it to the mail-forwarding-audit skill in m365-administration.
   - Settings-shaped (sharing, legacy auth, password policy) → metric reads on tenant/SharePoint/OneDrive sharing configuration and auth policy sections.
   - Open-ended ("what looks off in this tenant?") → liongard_query in natural language, then verify anything actionable with a targeted metric read.
3. Pull change context when the question is "did this change?": liongard_detection and liongard_timeline for admin-role grants, license count changes, and sharing-setting flips, with timestamps.
4. Output: the direct answer first, then the evidence rows, the dataprint as-of timestamp, and which fields were read. Post to the ticket as a plain-text note on request.

## Guardrails

- Never present dataprint data as live — always state the capture age; M365 changes minute-to-minute and Liongard sees it on inspector cadence.
- Verify all JMESPath field paths against the live dataprint before trusting a zero — an empty result from a wrong path looks identical to a true zero. If a path returns nothing, confirm the section exists before reporting "none."
- Read-only by definition: any change this read motivates routes to the matching m365-administration skill or a technician.
- Admin-role and forwarding findings are security-relevant even when nobody asked — surface them, flagged, rather than silently answering only the question.
- Degradation: no M365 launchpoint for this client → fall back to search_tickets history and docs (IT Glue/Hudu where enabled), and label the answer "not instrumented — attestation only."
