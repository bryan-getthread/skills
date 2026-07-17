---
name: Liongard M365 Tenant Read
description: Answer tenant-level Microsoft 365 questions from the Liongard M365 inspector's dataprint — license inventory and assignment, mailbox statistics, admin roles, secure-score inputs, and sharing settings — without touching the tenant.
category: Liongard Inspectors
tools: [liongard_launchpoint, liongard_metric, liongard_identity, liongard_detection, liongard_timeline, liongard_query]
connectors: [Liongard]
---

# Liongard M365 Tenant Read

**When to use:** "How many global admins are in <client>'s M365 tenant / who holds admin roles?", license counts and unassigned SKUs, mailboxes near quota or with forwarding set, external-sharing and secure-score-input questions — any M365 tenant fact where reading is enough.

## Prompt

```
Answer a tenant-level Microsoft 365 question for CLIENT_NAME from the Liongard Microsoft 365 inspector's dataprint. Read-only — any change this motivates routes to a technician or an m365-administration skill.

1. liongard_launchpoint filtered by systemType "Microsoft 365" for the client's environment. Confirm the launchpoint exists and the last run succeeded; capture the last-run timestamp — every answer carries "as of <timestamp>." A failed or ancient run means the answer is stale: say so and offer a technician-side refresh before anyone acts.
2. Route the question, verifying every JMESPath field path against the live dataprint (schemas vary by inspector version — an empty result from a wrong path looks identical to a true zero):
   - Identity-shaped (admins, MFA, stale accounts) → liongard_identity for the reconciled view, then liongard_metric for tenant-specific detail. Admin-role work hands off to a security audit for judgment; this supplies the raw reads.
   - License-shaped → liongard_metric against subscribed-SKU and per-user assignment sections: total vs. consumed per SKU, unassigned count, licensed-but-disabled accounts (money leaking). Example angles to verify against the live dataprint: `Skus[?SkuPartNumber=='SPE_E3'].{total: PrepaidUnits.Enabled, used: ConsumedUnits}`, `Users[?AccountEnabled==\`false\` && length(AssignedLicenses) > \`0\`]`.
   - Mailbox-shaped → metric angles over mailbox statistics: size vs. quota, item counts, forwarding addresses, litigation hold. A forwarding address outside the client's domains is a security lead, not a stat — flag it.
   - Settings-shaped (sharing, legacy auth, password policy) → metric reads on tenant/SharePoint/OneDrive sharing and auth-policy sections.
   - Open-ended ("what looks off?") → liongard_query, then verify anything actionable with a targeted metric.
3. "Did this change?" → liongard_detection and liongard_timeline for admin-role grants, license count changes, and sharing-setting flips, with timestamps.
4. Surface admin-role and external-forwarding findings even when unasked, flagged. Never present dataprint data as live — M365 changes minute-to-minute; state the capture age. If a path returns nothing, confirm the section exists before reporting "none."
5. Output: the direct answer first, then evidence rows, the dataprint as-of timestamp, and which fields were read. Post to the ticket as a plain-text note on request. Degradation: no M365 launchpoint → search_tickets history and docs (IT Glue/Hudu), labeled "not instrumented — attestation only."
```
