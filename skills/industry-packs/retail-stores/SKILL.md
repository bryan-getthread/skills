---
name: Supporting Retail Store Clients
description: Vertical pack for retail and multi-store clients — POS + inventory platforms (Lightspeed, Shopify POS, NCR Counterpoint-class), PCI boundary discipline, the seasonal change freeze (Black Friday through year-end), multi-site scope patterns, and e-commerce/in-store sync. Load when the client is a retailer or the ticket names a POS, barcode/label hardware, inventory sync, or a store network.
category: Industry Packs
tools: [search_tickets, search_knowledge_base, search_itglue, search_hudu, add_ticket_note, update_ticket, web_search]
---

# Supporting Retail Store Clients

Retail IT is the same small stack stamped across locations: POS lane, payment terminal, barcode and receipt hardware, back-office PC, store network. That replication is the vertical's defining diagnostic move — "is this one store or every store?" — and its defining calendar is the season: from Black Friday to year-end, the stores make the year, and nothing discretionary changes. This pack layers retail knowledge onto the LOB Application Framework (troubleshooting-playbooks/lob-application-framework).

## When to use

- The client is a retail store, chain, or omnichannel merchant, or the ticket names Lightspeed, Shopify POS, Square Retail, NCR Counterpoint, Heartland/pcAmerica, RICS, Celerant-class systems
- "Registers are down," "cards aren't processing," "inventory counts are wrong," "online and in-store stock don't match"
- Barcode scanner, label/receipt printer, cash drawer, or customer-display tickets
- Store-opening, store-network, or new-lane rollouts; anything scheduled near the holiday season
- Loss-prevention adjacent requests (cameras, exception reports)

## The stack you'll meet

- **POS + inventory:** Lightspeed, Shopify POS, Square, NCR Counterpoint, Heartland, RICS, Celerant-class — sales, SKUs, stock levels, purchasing, and customer records in one platform, mostly cloud with an offline-capable lane app in the better cases. Inventory integrity is the retailer's second heartbeat after payments: sync failures between POS, e-commerce (Shopify/WooCommerce-class), and warehouse systems silently oversell stock — treat "counts are off" as an integration investigation, not user error, and find *when* the numbers diverged.
- **Payments:** processor-managed terminals on a P2PE or semi-integrated path. Terminal swaps and pairing follow the processor's documented procedure; a payments-only failure (POS fine, cards declining) points at the terminal, its network path, or the processor — check the processor status page early.
- **Store hardware:** barcode scanners (USB/Bluetooth pairing churn), receipt and label printers (the highest-volume ticket class; spare-on-shelf is the honest fix), cash drawers keyed to receipt printers, customer displays, and the back-office PC that runs reports and receiving.
- **Store network:** a small firewall/router, a switch, POS VLAN, guest wifi, cameras — replicated per site, ideally from a standard template. Deviations from the template are where mystery tickets live; flag undocumented drift when you find it.

## PCI boundary discipline

The cardholder-data environment is the POS/payments segment. Keep it clean: guest wifi, cameras, signage, and random devices stay off the POS VLAN; remote-access tools into store networks follow the documented pattern only; no card numbers in tickets, notes, or screenshots ever. Suspected skimming, terminal tampering, or a processor breach notice → security incident path immediately, owner/LP notified — evidence preserved, terminal taken out of service per processor guidance, no routine-ticket handling.

## The rhythm and what's sacred

- **The seasonal freeze:** from mid-November (Black Friday) through the New Year, stores are in peak revenue and support runs "break-fix only" — no rollouts, migrations, network changes, or version upgrades unless something is down. Plan seasonal-hardware readiness (spare printers, scanners, terminals staged) *before* November; flag any project someone tries to schedule into the freeze.
- **Weekend peaks:** Saturday is the biggest floor day; register or payments failure Saturday morning is the week's worst-case.
- **Opening hour:** registers must open — a lane that won't sign in at 9:55 AM is urgent even if three others work.
- **Sacred:** the payment path, the POS platform, inventory-sync integrity during season, and the store-network template.

## Steps

1. Context: search_tickets for this retailer's history (printer and pairing fixes repeat; check whether this exact issue hit another store last week), and search_itglue / search_hudu for the POS platform, processor, store-network template, per-store deviations, e-commerce integrations, and spare-hardware locations.
2. Scope first — one store or many? Before deep-diving any store-down ticket at a multi-site client, check sibling stores and the vendor status page. Fleet-wide symptoms mean platform/vendor or template-level causes; single-store means local network or hardware. State the scope finding in the ticket.
3. Set severity on the retail clock: payments or all-lanes failure during open hours → highest priority (worse in season/weekends); single-lane or single-peripheral failure with others working → normal, load shifted, spare deployed.
4. Payments failures: split POS-app vs terminal vs processor. Follow the processor's documented procedures for terminal resets/swaps; log the processor case when it's upstream; never improvise in the payment path.
5. Inventory/sync issues: run the LOB framework with an integration lens — identify the sync direction and schedule, when divergence began, and what changed then (platform update, integration re-auth expiry, SKU imports); vendor-escalation package with case number when it's inside the platform.
6. Note in plain text: system, store(s) affected, retail-clock impact, scope finding (one vs many), error verbatim (no card data), branch taken, spare/workaround deployed, vendor case, and verification — a real sale rung and settled, a real label printed, counts spot-checked.

## Guardrails

- Honor the seasonal freeze: break-fix only from Black Friday through year-end; escalate any project requested inside the window to the account owner rather than quietly doing it.
- PCI: the POS segment stays clean, card numbers never in tickets, terminal work per processor procedure only; suspected tampering/skimming → security incident path, evidence preserved, immediately.
- Always answer "one store or many?" before deep local debugging at a multi-site client — and say which in the ticket.
- Never bulk-adjust inventory counts to "make them match" — find the divergence cause; count corrections are the retailer's decision with their process.
- Store-network changes follow the documented template; ad-hoc deviations create the next mystery ticket — flag drift you find.
- Loss-prevention data (cameras, exception reports) is sensitive — access changes go through the documented owner, not the requesting store manager alone.
- Docs tools vary per tenant — state what you could not verify; missing store-template documentation or unstaged seasonal spares are flags worth raising before November.
