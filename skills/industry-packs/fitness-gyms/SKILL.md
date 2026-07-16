---
name: Supporting Fitness and Gym Clients
description: Vertical pack for gym, studio, and fitness-franchise clients — membership/booking platforms (Mindbody, ABC Fitness-class), door access tied to membership status at 24/7 locations, billing-draft day sensitivity, licensed music/AV systems, and the early-morning peak. Load when the client is a gym or studio, or the ticket names a membership platform, door access, check-in kiosks, or class booking.
category: Industry Packs
tools: [search_tickets, search_knowledge_base, search_itglue, search_hudu, add_ticket_note, update_ticket, web_search]
---

# Supporting Fitness and Gym Clients

A gym's stack is small but tightly coupled: the membership platform decides who may enter, the door controller enforces it at 5 AM when no staff are on site, and the billing draft pulls hundreds of small payments on a schedule. The failure that defines this vertical is a member standing at a locked door in the dark because an integration hiccuped. This pack layers fitness-vertical knowledge onto the LOB Application Framework (troubleshooting-playbooks/lob-application-framework).

## When to use

- The client is a gym, fitness studio, franchise location, climbing/swim facility, or boutique-fitness operation, or the ticket names Mindbody, ABC Fitness/Ignite, Mariana Tek, Glofox, Zen Planner, ClubReady, PushPress-class platforms
- "Members can't get in," fob/QR/app door-entry failures — especially at unstaffed hours
- Check-in kiosk, class-booking, or member-app tickets
- Billing/draft-day issues, POS at the front desk or smoothie bar
- Music/AV, screens, or cardio-equipment network tickets

## The stack you'll meet

- **Membership/booking platform:** Mindbody, ABC Fitness, Mariana Tek, Glofox, Zen Planner, ClubReady-class — members, plans, class schedules, and billing live here, nearly always cloud. Many "gym system down" tickets are vendor-side or internet-side; check the vendor status page early and say so plainly.
- **Door access tied to membership:** 24/7 access gyms wire the door controller to membership status — a fob/QR/app scan checks the platform (live API or a cached/synced allowlist). Failure modes split three ways: platform/integration down (nobody gets in), sync stale (recently joined members bounce), or hardware (reader, controller, door strike). Know which pattern this site uses and whether the controller has an offline/cached mode — that determines how bad a platform outage really is at 5 AM.
- **Front of house:** check-in kiosks/tablets, front-desk POS, receipt printers, member-photo cameras. Kiosk lockups at peak create real lines.
- **The floor:** music systems on *licensed commercial* streaming services (a licensing/compliance matter — never "fix" broken music by substituting a consumer streaming account; flag licensing questions to the owner), TVs/signage, networked cardio equipment and their vendor-managed backends, and group-fitness AV (a dead mic cancels a class in practice).

## Payments and PCI

Membership billing runs as recurring drafts through the platform's payment stack; the front desk adds card-present POS. Standard PCI discipline applies: card-handling devices stay on their proper segment, no card numbers ever land in tickets or notes, and payment-hardware swaps follow the processor's documented procedure. Draft-day failures are money events — the owner hears about them fast, so escalate honestly and early.

## The rhythm and what's sacred

- **The 5–8 AM peak:** the heaviest, least-staffed hours. Door-access or check-in failure then means members locked out with no staff to let them in — top severity at hours most desks consider quiet. The 5–8 PM peak is second, but staffed.
- **Draft day:** the monthly billing draft (often the 1st, or per-member anniversary) is sacred — no discretionary changes to the platform, payment integrations, or network on and immediately before draft runs. A botched draft is hundreds of failed payments and a support flood.
- **January and challenge launches:** new-member surges stress kiosks, onboarding flows, and door sync; expect volume.
- **Sacred:** the door-access chain (platform → sync → controller → strike) at 24/7 sites, the billing draft, and class-booking before peak class blocks.

## Steps

1. Context: search_tickets for this gym's history (reader reboots and sync-lag fixes repeat), and search_itglue / search_hudu for the platform, door-access vendor and integration pattern (live vs cached), controller locations, draft schedule, franchise-vs-independent status, and after-hours escalation.
2. Set severity on the gym clock: members locked out at unstaffed hours → top severity, immediate; whole-site platform outage during peak → high, with the vendor status checked first; single kiosk/screen/music issue → normal with a workaround stated.
3. Door-access tickets — split the chain in order: is it all members or only recent joins (sync lag)? Does the platform show an outage? Is the reader/controller powered and online (PoE, controller LEDs)? Hardware faults at the strike/reader are dispatch or access-vendor territory; integration faults are platform-vendor territory — open the right case and log it.
4. Run the LOB framework for platform/app failures: plan/version, change correlation, verbatim error, scope, vendor-escalation package with case number. Franchise locations: confirm who may approve changes — the franchisor may mandate the platform and its configuration, so route config-change requests to the documented approver.
5. Billing/draft issues: never retry, re-run, or modify drafts or payment settings yourself — gather the facts (error, scope, timing) and put the platform vendor and the owner/manager together with a case number; the desk stays out of the money path.
6. Note in plain text: system, gym-clock impact (locked-out members yes/no), scope, error verbatim (member-PII-scrubbed — no member names paired with payment details), branch taken, vendor case, verification — a member badge-in and a kiosk check-in performed for real.

## Guardrails

- Locked-out members at unstaffed hours are top severity regardless of the hour — treat 5 AM door failures like business-hours outages.
- Never operate on billing drafts or payment configuration — facts to the vendor and owner; the desk is not in the money path.
- Draft day and the day before are a change-freeze window for the platform, payments, and network.
- Never substitute consumer music accounts for licensed commercial audio — flag music-licensing questions to the owner.
- No member PII paired with payment details in tickets; PCI discipline on anything card-adjacent.
- At franchise locations, platform and config changes go through the documented franchise approver — the franchisor may own decisions the local manager cannot make.
- Docs tools vary per tenant — state what you could not verify; an undocumented door-access integration pattern (live vs cached) is a flag worth raising before the next 5 AM outage.
