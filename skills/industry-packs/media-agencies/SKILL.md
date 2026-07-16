---
name: Supporting Media and Creative Agencies
description: Vertical pack for creative, marketing, and production-agency clients — Adobe CC licensing pain, huge-file video/design workflows (NAS, proxies, review platforms), freelancer access churn, and client-brand asset security under NDA. Load when the client is an agency or studio, or the ticket names Adobe apps, a NAS/asset server, render issues, or freelancer onboarding.
category: Industry Packs
tools: [search_tickets, search_knowledge_base, search_itglue, search_hudu, add_ticket_note, update_ticket, web_search]
---

# Supporting Media and Creative Agencies

An agency's inventory is files — terabytes of client-branded, NDA-covered, deadline-bound files — worked on by a shifting cast of staff and freelancers in expensive per-seat software. The recurring tensions are licensing (who has a seat right now), storage (where the huge files live and how fast), access churn (who should still be able to touch client X's assets), and the pitch-day clock. This pack layers agency knowledge onto the LOB Application Framework (troubleshooting-playbooks/lob-application-framework).

## When to use

- The client is a creative, marketing, advertising, video-production, or design agency/studio, or the ticket names Adobe Creative Cloud apps (Premiere, After Effects, Photoshop, InDesign, Illustrator), DaVinci Resolve, Cinema 4D, Figma, or a review platform (Frame.io-class)
- "I don't have a license," "Creative Cloud signed me out," seat-assignment and license-reclaim requests
- NAS/asset-server performance, sync, or capacity tickets; "the project file is corrupt"; render or export failures
- Freelancer onboarding/offboarding, or client-asset access-scoping questions

## The stack you'll meet

- **Creative apps and licensing:** Adobe CC named-user licensing via the Admin Console is the center of gravity — seats are per-user, finite, and expensive, so agencies constantly reassign them as freelancers rotate. "License not found" tickets are usually seat-assignment, sign-in-profile (personal vs business Adobe ID), or seat-exhaustion issues; know who at the agency approves buying vs reassigning a seat. Adjacent per-seat pain: font licensing (Adobe Fonts vs third-party — missing-font tickets on handoff are constant), plugin licenses, and stock-asset accounts.
- **Storage — the huge-file problem:** a NAS/SAN (Synology, QNAP, LumaForge-class) or increasingly cloud (Dropbox/Drive with selective sync, LucidLink-class) holds active projects; video work adds proxy workflows and scratch disks. Classic failures: sync clients choking on multi-GB files, editors working off the wrong copy, full scratch disks masquerading as "Premiere is broken," and NAS capacity creep. Archive/backup discipline for finished projects is chronically weak — confirm what's actually protected before assuming.
- **Review and delivery:** Frame.io-class review links, WeTransfer-class delivery, client portals. Broken review links on approval day are top-severity in agency terms.
- **Hardware:** color-managed displays (calibration matters — don't casually reset display profiles), GPU-heavy edit bays, render nodes, audio interfaces.

## Access churn and client-asset security

- **Freelancers:** rapid on/offboarding is the norm — contractor accounts, temporary CC seats, and scoped access to only the client folders they need. Offboarding must reclaim the CC seat, revoke storage and review-platform access, and remove them from client channels — same-week at latest, same-day when the agency says the engagement ended. A departed freelancer with lingering NAS access is a client-confidentiality incident waiting to happen.
- **Client-brand assets:** unreleased campaigns, embargoed launches, and brand assets sit under NDA. Scope access per client, not "everyone sees everything," where the agency's structure supports it; never paste asset contents, campaign names paired with clients, or embargoed material into tickets. A leak here loses the agency the account.
- Suspected asset exposure (public share link on an embargoed folder, ex-freelancer access used) → capture facts, flag the agency principal; no legal advice.

## The rhythm and what's sacred

- **Pitch and launch days:** the days before a pitch or campaign launch are the fragility peak — no discretionary changes to storage, licensing, or review platforms then; ask "when is this due?" during triage, because a small ticket the day before a pitch is a big ticket.
- **Overnight renders:** long renders and uploads run overnight; a workstation update policy that reboots mid-render burns a day — schedule patching around render habits.
- **Sacred:** the active-projects volume and its backup, the CC Admin Console integrity, and review/delivery paths near deadlines. Losing an active project file to sync conflict or corruption is losing billed hours.

## Steps

1. Context: search_tickets for this agency's history (seat shuffles and sync-conflict fixes repeat), and search_itglue / search_hudu for the storage architecture, CC Admin Console ownership, seat count and approver, freelancer on/offboarding checklist, and backup/archive scope.
2. Triage on the deadline clock: ask what's due and when. Whole-team storage or review-platform failure near a pitch/launch → top severity; single-seat licensing issue → fast but routine (seat reassignment beats waiting on a purchase).
3. License tickets: confirm which Adobe ID profile the user signed into (personal vs business is the classic), check seat assignment in the Admin Console context from docs, reclaim idle seats per the agency's rule before requesting purchases — purchases go to the documented approver.
4. Storage/file tickets: establish where the file truly lives (NAS vs sync copy vs local scratch), check free space on scratch/cache disks early, and treat sync conflicts as potential data-loss events — preserve both copies before "fixing." Corrupt project files → check the app's autosave/version folder and the backup before anything destructive.
5. Run the LOB framework for app failures: exact versions (creative-app + OS + GPU driver combinations matter — check the vendor's certified/known-issue matrix before driver updates), change correlation, verbatim error, vendor case when it's vendor territory.
6. Freelancer offboarding: run the documented checklist — CC seat, storage, review platforms, email/channels, VPN — same-day when directed; record each revocation in the ticket.
7. Note in plain text: system + versions, deadline context, scope, error verbatim (client/campaign names scrubbed where possible), branch taken, vendor case, verification — the user opens/renders/uploads the real project, not just app-launches.

## Guardrails

- Preserve before you fix: sync conflicts, "corrupt" project files, and full-disk cleanups all get copies safeguarded first — active project files are billable work product.
- Never reset display color profiles or update GPU drivers on edit/color machines ad hoc — check vendor-certified combinations and the user's calibration setup first.
- Freelancer offboarding is checklist-complete and prompt; lingering access to client assets is a confidentiality incident, not a cleanup task.
- No embargoed campaign material, client-brand pairings, or asset contents in tickets beyond minimum necessary.
- License purchases and plan changes go to the agency's documented approver — reassign idle seats first, never buy on your own initiative.
- Deadline-adjacent freeze: no discretionary storage/licensing/platform changes in the window before a stated pitch or launch.
- Docs tools vary per tenant — state what you could not verify; weak archive/backup coverage of finished projects is a flag worth raising as its own follow-up.
