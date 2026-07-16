---
name: Supporting Restaurants and QSR Clients
description: Vertical pack for restaurant, QSR, and multi-unit food-service clients — the POS core (Toast, Square, Aloha/NCR-class), kitchen display and online-ordering integrations, the franchise-vs-corporate IT split, PCI discipline, and the service-rush no-touch windows. Load when the client is a restaurant or food-service operator, or the ticket names a POS, KDS, or delivery-platform integration.
category: Industry Packs
tools: [search_tickets, search_knowledge_base, search_itglue, search_hudu, add_ticket_note, update_ticket, web_search]
---

# Supporting Restaurants and QSR Clients

A restaurant's IT is its POS, and its POS matters most exactly when nobody can look at a ticket: the lunch and dinner rushes. The desk's discipline in this vertical is temporal — know the no-touch windows, know the offline fallbacks, and know whether the person calling is even allowed to approve a change (franchisees often aren't). This pack layers restaurant knowledge onto the LOB Application Framework (troubleshooting-playbooks/lob-application-framework).

## When to use

- The client is a restaurant, QSR/fast-casual operation, franchise group, bar, café, or ghost kitchen, or the ticket names Toast, Square for Restaurants, Aloha/NCR, Micros/Simphony, Clover, SpotOn, Revel, Lightspeed Restaurant-class POS
- "The POS is down," "cards aren't going through," "orders aren't reaching the kitchen"
- Online-ordering and delivery-platform issues (DoorDash/UberEats/Grubhub tablets or integrated injection), KDS failures
- Franchise-location tickets where approval authority is unclear
- Back-office, inventory, scheduling, or payroll-adjacent restaurant systems

## The stack you'll meet

- **POS core:** Toast, Square, Aloha/NCR, Micros/Simphony, Clover, SpotOn, Revel-class — terminals, handhelds, payments, menu, and reporting. Cloud POS platforms usually have an offline mode that keeps taking card-present orders through an outage (syncing later) — know whether this site's POS has it, what it covers (dine-in yes, online no), and where the vendor's offline procedure is documented, because that determines whether an internet outage is an inconvenience or a closure.
- **Kitchen chain:** orders flow POS → kitchen printers and/or KDS screens. "Orders aren't printing in the kitchen" during service is as severe as POS-down — food stops. Kitchen hardware lives in heat and grease; printer and screen failures are chronic, and spare-on-shelf is the honest fix.
- **Online ordering and delivery:** either standalone vendor tablets (DoorDash/UberEats tablets lined up at expo) or integrated order-injection into the POS (via the platform's own integration or a middleware layer). Integration failures strand orders in limbo — customers charged, kitchen unaware — so treat "online orders stopped arriving" as urgent and check both the POS vendor's and the delivery platform's status pages before local debugging.
- **Adjacent:** payment terminals (processor-managed), back-office PC (inventory/food-cost, scheduling like 7shifts/HotSchedules-class), drive-thru timers and headsets at QSR, guest wifi (segmented from POS — see PCI), music, and cameras.

## The franchise-vs-corporate split

At franchised brands, the franchisor typically mandates the POS platform, menu structure, payment processor, and sometimes the network stack; the franchisee owns the local hardware, internet, and day-to-day operation. Practical consequences: the GM on the phone may not be allowed to approve a config change; some fixes can only be made by corporate or the brand's designated vendor; and support boundaries (who fixes the POS vs the network vs the delivery tablets) should be in docs. Establish "who may approve what" early, route brand-mandated-system changes to the documented approver, and never modify brand-standard configurations on a franchisee's say-so alone.

## PCI discipline

Card data flows through the POS and payment terminals. Keep the POS/payments segment separate from guest wifi and back-office; never put random devices on the POS VLAN; payment-terminal swaps and P2PE hardware follow the processor's documented procedure; and card numbers never appear in tickets, notes, or screenshots. Suspected card-data compromise (skimmer, breach notice, processor alert) → security incident path immediately, owner notified — not a routine ticket.

## The rhythm and what's sacred

- **No-touch windows:** roughly 11:30 AM–1:30 PM and 5:30–9:00 PM (later for bars, all-day weekends for brunch spots) are service rushes — no reboots, no updates, no config changes, no "quick tests" on POS, KDS, network, or payments. The maintenance window is the closed period or mid-afternoon lull, and the manager tells you which.
- **Friday/Saturday dinner** is the week's revenue peak; anything degraded going into it deserves proactive attention Thursday.
- **Sacred:** the POS-to-kitchen chain and the payment path during service, and the offline-fallback capability itself (an untested offline mode is a false sense of safety).

## Steps

1. Context: search_tickets for this location's history (kitchen-printer and tablet fixes repeat), and search_itglue / search_hudu for the POS platform and hosting model, offline-mode capability, delivery integrations, franchise status and approval matrix, processor, and the site's stated service hours.
2. Set severity on the service clock: POS, payments, or kitchen-chain failure during service → highest priority, immediate; the same failure at 3 PM → high but plannable; single terminal down with others working → normal with load shifted.
3. During-service outages: stabilize first, diagnose later — get the site onto its documented offline/fallback procedure (offline card mode, paper tickets to kitchen) and defer root-cause work to after the rush; note what was deferred.
4. Split the failure domain: terminal vs local network vs internet vs POS-vendor cloud vs payment processor vs delivery platform. Check vendor status pages early — this vertical is cloud-heavy and many outages are upstream; say so plainly and log the vendor case.
5. Run the LOB framework for platform issues: versions/plan, change correlation (menu pushes and POS updates landing mid-day are classics), verbatim error, scope (one terminal / one station / whole site / multiple sites — multi-site scope at a group means check siblings immediately), vendor-escalation package with case number. Route approval-required changes per the franchise matrix.
6. Note in plain text: system, service-clock impact, scope, error verbatim (no card data ever), fallback used, branch taken, vendor case, approver, and verification — a real order rung, paid, and fired to the kitchen after the fix.

## Guardrails

- Respect the no-touch windows absolutely: no discretionary changes to POS, payments, KDS, or network during service; stabilize-then-defer is the during-rush posture.
- Never modify brand-mandated configurations without the documented franchise approver; the GM's urgency does not confer authority.
- PCI: card numbers never in tickets; POS segment stays clean; terminal hardware follows processor procedure; suspected card-data compromise → security incident path and owner, immediately.
- Never leave an online-ordering integration failure "for the morning" — stranded paid orders are refund and reputation damage by the hour.
- Know the offline fallback before you need it: if docs show none, flag that as its own follow-up; do not improvise payment workarounds.
- Multi-site groups: check whether siblings share the failure before treating any site-down as local.
- Docs tools vary per tenant — state what you could not verify; an undocumented approval matrix at a franchise client is a flag worth raising.
