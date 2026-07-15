---
name: Supporting Churches and Faith Organizations
description: Vertical pack for church, synagogue, mosque, and ministry clients — the ChMS/giving stack (Planning Center, Pushpay-class), the streaming/AV rig, volunteer-heavy access churn, donation-data sensitivity, and the Sunday-morning sacred window. Load when the client is a faith organization or the ticket names worship AV, livestreaming, a church-management system, or a giving platform.
category: Industry Packs
tools: [search_tickets, search_knowledge_base, search_itglue, search_hudu, add_ticket_note, update_ticket, web_search]
---

# Supporting Churches and Faith Organizations

A church's IT peaks for two hours a week, run largely by volunteers who rotate every season, on systems holding some of the most sensitive data any small org keeps: who gave what, who asked for prayer about what, who attends. This pack layers faith-org knowledge onto the LOB Application Framework (troubleshooting-playbooks/lob-application-framework).

## When to use

- The client is a church, synagogue, mosque, temple, ministry, diocese, or faith-based nonprofit, or the ticket names Planning Center, Pushpay/ChurchStaq, Breeze, Rock RMS, Tithe.ly, Subsplash, ProPresenter, or a livestream rig
- Streaming/AV failures — especially with a service approaching
- Volunteer or staff access requests, departures, or shared-account questions
- Anything touching giving/donation data or platforms

## The stack you'll meet

- **ChMS (church management):** Planning Center (the modular default), Breeze, Rock RMS, Pushpay/ChurchStaq, Elvanto-class — members, groups, check-ins, volunteer scheduling, and service planning live here. Children's-ministry **check-in stations** (iPads/label printers) are the Sunday-morning flashpoint: they exist for child safety (matching pickup tags), so a check-in failure is a safety-process failure, not a printer ticket.
- **Giving:** Pushpay, Tithe.ly, Planning Center Giving, Subsplash Giving-class, plus legacy ACH and text-to-give. Donation data is the org's most sensitive dataset — giving records are confidential even *within* the org (typically only the finance team and sometimes the senior pastor see them; helpdesk work must not widen that circle). Payment flows are PCI territory: configure per vendor documentation, never handle or store card data.
- **The AV/streaming rig:** presentation software (ProPresenter-class), audio consoles (Behringer X32/Allen & Heath-class), video switchers (ATEM-class), cameras, and encoders streaming to YouTube/Facebook/Boxcast-class. It is a small broadcast studio maintained by volunteers — documentation and labeled, photographed known-good states are worth more here than anywhere, because the person operating it Sunday may have been trained for twenty minutes.
- **Also:** office M365/Google Workspace (nonprofit-granted licensing — the nonprofit pack's licensing rules apply: know the grant source, never assign outside permitted scope), website/app, Wi-Fi sized for Sunday crowds, and facility systems (door access, thermostats) that creep into scope.

## Access churn: the volunteer reality

- Volunteers run AV, check-in, social media, and sometimes the books — arriving and leaving with the seasons, using personal emails and personal devices. The failure mode is accretion: five years of ex-volunteers still in the ChMS, a shared "media@" password on a sticky note at the sound desk, a departed treasurer still on the bank-adjacent email.
- The posture: named accounts over shared ones wherever the platform's licensing allows (where a shared station account is unavoidable — the AV computer — document it, MFA it where possible, and rotate it on volunteer turnover); every volunteer grant gets a role-scoped permission and a recorded review date; departures get real offboarding same-week. Propose a **semi-annual access review** timed to volunteer-season turnover (fall kickoff, post-Easter) — small orgs will not think of it themselves.
- Ministry-role sensitivity: pastoral-care and counseling notes in the ChMS are clergy-confidential; giving data is finance-confidential. Permission requests widening access to either go to the org's designated leader (executive pastor/administrator), not granted on request.

## The rhythm and what's sacred

- **Sunday morning (or the org's holy day) is the sacred window.** The stream, the presentation machine, the audio console, and check-in must work at service time, and there is no do-over. Consequences for the desk: **no changes to anything service-critical from Friday through the last service** — no updates, no "quick improvements" to the streaming encoder on Saturday night. Pre-flight, don't patch.
- Support availability expectations: a 9 AM Sunday outage with services at 9:30 is this vertical's line-down — know what the support agreement actually promises on weekends and make sure the org knows too; an honest scope conversation beats a missed service.
- **Seasonal peaks:** Christmas/Easter (or the tradition's equivalents) multiply attendance, streaming load, and check-in volume — pre-flight capacity, freeze harder. **Year-end giving** (late December) makes the giving platform and giving-statement generation (January) sacred, exactly like the nonprofit pack's giving season.
- OS/app updates are the classic Sunday-killer: a presentation machine that auto-updated Saturday night. Manage update timing deliberately on service-critical machines — updates happen Monday–Thursday, deferred otherwise.

## Steps

1. Context: search_tickets for this org + system history (AV and check-in tickets recur seasonally with known fixes), and search_itglue / search_hudu for documentation: ChMS and giving platforms, license grant sources, the AV rig inventory and its known-good state, service schedule, designated approver for access and spending.
2. Triage on the service clock: anything service-critical failing within ~48 hours of a service → urgent, with the goal of *restore known-good* (swap the spare, revert the update, use the documented fallback) rather than root-causing under the gun — root cause happens Monday. Check-in failures during services get the manual child-safety fallback communicated first, diagnosis second.
3. For access tickets: apply the churn posture — named account, role-scoped, review date recorded, leader approval for anything touching giving data or pastoral notes; departures offboarded same-week including shared-credential rotation at stations they used.
4. For giving-platform issues: vendor status page early (cloud-heavy), never touch card data, and keep giving details out of tickets — amounts, donors, and fund designations are minimum-necessary at all times; suspected exposure → facts + flag org leadership immediately.
5. Run the LOB framework for app failures: exact versions/plan tier, change correlation (auto-updates above all), verbatim error, vendor known-issue search, vendor-escalation package when it's platform territory — with the next service date stated in the case.
6. Improve resilience opportunistically: after any AV incident, update the documented known-good state (photos of console scenes, encoder settings, cable maps) so the next volunteer can restore it — the documentation *is* the fix in a volunteer-run rig.
7. Note in plain text, giving-and-pastoral-data-free: system, service-clock context, restore-vs-diagnose decision, approvals, error verbatim, vendor case, verification at the next real service or a full rehearsal-mode test.

## Guardrails

- Service-window freeze: no changes to stream, presentation, audio, or check-in systems from Friday through the last service; manage auto-updates deliberately on those machines.
- Giving data is confidential even inside the org — never widen access without the designated leader's approval, never paste giving details into tickets, never handle card data (PCI: vendor-documented procedures only).
- Pastoral-care and counseling data in the ChMS is clergy-confidential — access requests route to leadership; contents never appear in tickets.
- Check-in failures are child-safety process failures: communicate the manual fallback to the org before diagnosing.
- Volunteer access is named, role-scoped, and expiry-dated; shared station accounts are documented exceptions with rotation on turnover — never the default.
- Nonprofit-granted licenses follow their grant scope (see the nonprofits pack); budget sensitivity applies — check charity pricing and free paths before recommending spend.
- Be honest about weekend support scope against the actual agreement; escalate scope gaps to the account owner rather than silently absorbing or silently failing.
- Docs tools vary per tenant — state what you could not verify; an org with no documented AV known-good state or access approver is itself a flag worth raising.
