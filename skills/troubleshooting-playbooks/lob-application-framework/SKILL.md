---
name: LOB Application Framework
description: The generic playbook for any line-of-business application failure — dental/legal/accounting/ERP or anything vertical — identify the vendor and version, get the log, search known issues, and build a complete vendor-escalation package.
category: Troubleshooting Playbooks
tools: [search_tickets, search_knowledge_base, search_itglue, search_hudu, add_ticket_note, web_search]
---

# LOB Application Framework

No desk knows every vertical application, and it doesn't have to. Every LOB ticket answers the same six questions: what app and version, what changed, what does the error/log say, is it one user or all, has the vendor already documented this, and — when the desk can't fix it — is the vendor escalation package complete enough that the vendor can. This framework is that sequence.

## When to use

- Any application the desk doesn't have a specific playbook for — practice management, ERP, legal, accounting, industry software
- "<app> won't open / errors / lost connection to its database"
- LOB app broke after an update (its own, or Windows)
- Deciding what the desk can fix vs what goes to the app vendor

## Steps

1. **History first.** search_tickets for this app at this client — LOB apps generate repeat tickets with known local fixes (a service restart, a workstation re-link), and the prior ticket is the fastest resolution path. Also check search_knowledge_base for a client-specific procedure.
2. **Docs second.** search_itglue / search_hudu for the app's documentation at this client: vendor and support contract details, server/database location, install/config notes, vendor portal credentials *location* (never the credentials themselves into the ticket), and any vendor-supplied runbooks.
3. **Identify the software precisely — never assume.** Vendor, product, exact version/build (Help → About, or the file properties), server vs client components and both their versions, and the database platform underneath if any. Client/server version mismatch after a partial update is a top LOB failure — comparing the two is step one, not step five.
4. **Scope and change correlation.** One user or all users? One workstation or everywhere? What changed in the window — app update, Windows patches, AV/EDR deployment, server maintenance, expired app licensing? LOB apps are disproportionately broken *by* their environment (a patched workstation, a renamed server, a security agent quarantining an unsigned component): the change answers most tickets.
5. **Get the log/error before theorizing.** The exact error text (screenshot or copied verbatim — LOB error strings are searchable gold), plus the app's own log (ask the vendor docs where it lives) and the Windows Application event log at the failure time. Do not proceed on "it errors out."
6. **Known-issue search.** web_search the exact error string plus product and version, and the vendor's support/KB site specifically. A documented known issue with a vendor fix or workaround ends the diagnosis. Judge source quality: the vendor's KB and release notes outrank forum folklore — never present a forum workaround as vendor guidance.
7. **Branch on what the desk can own:**
   1. **Environment-side** (the desk's to fix) — connectivity to the app server/database, permissions on shares/folders the app uses, security-agent interference (check quarantine/exclusion state against the vendor's documented exclusions — exclusion *changes* route to the security owner, not done casually), workstation component reinstall/repair per vendor procedure.
   2. **Application-side** (vendor territory) — defects in the app, database corruption in its schema, licensing server faults, version-upgrade problems. Be plain: the desk does not patch vendor code or surgery vendor databases; attempting it risks the client's data and the vendor's support standing. Go to step 8.
   3. **Update-correlated** — if the vendor's own update broke it, check the vendor's guidance for rollback (never improvise a rollback of an app that owns a database — version-mismatched data is worse than downtime); if a Windows patch broke it, the interim may be the vendor's documented compatibility fix, and only the vendor can ship the real one — say so.
8. **Vendor-escalation package — the framework's real product.** When it goes to the vendor, send a case they can act on immediately: product and exact versions (client and server), OS versions, the verbatim error, the app log excerpt at failure time, scope (who/how many/since when), the change correlation, what the desk already tried and results, business impact, and the client's support-contract identifiers (from docs). Open it through the client's entitled channel, capture the case number in the ticket, and set the follow-up cadence. A complete package saves the round-trips that make LOB tickets take weeks.
9. **Verify and note.** The user performs the actual failing workflow (not just app-opens). Plain-text note: app/version, scope, change correlation, error, branch, known-issue findings with source, action or vendor case number, verification.

## Guardrails

- Never operate on an LOB application's database or files outside the vendor's documented procedures — data integrity and support-contract standing are both at stake; say plainly when only the vendor can act.
- Version facts are collected, never assumed — and known-issue findings cite their source, with vendor documentation outranking forums; do not invent KB articles or fixes.
- Credentials for vendor portals stay in the documentation system — reference their location, never paste them into tickets.
- Security-agent exclusions for an app are changes owned by the security policy owner, made per the vendor's published exclusion list — not ad-hoc troubleshooting.
- No remote execution — all steps are guidance for the tech; the vendor package is the deliverable when the desk's branch ends.
- Docs tools vary per tenant — note what you could not check, and flag missing LOB documentation (no vendor contact, no versions recorded) as its own follow-up.
