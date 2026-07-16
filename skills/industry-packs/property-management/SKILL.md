---
name: Supporting Property Management Clients
description: Vertical pack for property-management clients — PM platforms (Yardi, AppFolio, Buildium-class), tenant-portal and rent-day sensitivity, owner-vs-tenant data separation and the support-scope boundary, maintenance-request integrations, and trust-accounting caution. Load when the client is a property manager or the ticket names a PM platform, tenant portal, rent payments, or maintenance workflows.
category: Industry Packs
tools: [search_tickets, search_knowledge_base, search_itglue, search_hudu, add_ticket_note, update_ticket, web_search]
---

# Supporting Property Management Clients

A property manager sits between two populations it doesn't employ — owners whose money it holds in trust, and tenants who pay rent and file maintenance requests through a portal. The platform that mediates all of it is the business, rent week is its heartbeat, and the desk's classic traps are scope (tenants calling the MSP's client's help desk) and money (touching anything near trust accounting or payment routing). This pack layers PM knowledge onto the LOB Application Framework (troubleshooting-playbooks/lob-application-framework).

## When to use

- The client is a residential/commercial property manager, HOA/community-association manager, or student-housing operator, or the ticket names Yardi (Voyager/Breeze), AppFolio, Buildium, RealPage, Rent Manager, Entrata, Propertyware, DoorLoop-class platforms
- "Tenants can't pay online," tenant-portal login floods, rent-day platform slowness
- Maintenance-request workflow issues (portal → work order → vendor dispatch chain)
- Owner-portal, owner-statement, or owner-draw timing questions
- Anything adjacent to trust accounting, payment routing, or bank-detail changes

## The stack you'll meet

- **PM platform:** Yardi, AppFolio, Buildium, RealPage, Rent Manager, Entrata-class — properties, leases, tenants, owners, work orders, and accounting in one system, nearly all cloud (Yardi Voyager may be hosted enterprise). Most platform outages are vendor-side: check the vendor status page early and say so plainly.
- **Tenant portal:** rent payment, maintenance requests, lease documents. Portal failures generate ticket floods because the affected population is every tenant, not every employee — a portal-payment outage in rent week is the vertical's signature emergency.
- **Maintenance chain:** tenant request → work order → assignment to in-house techs or vendors (sometimes through integrations or add-ons like Property Meld-class) → completion and billing. A silent integration failure strands urgent requests — a tenant with a burst pipe whose request went nowhere is both a service failure and a liability event for the PM.
- **Adjacent:** owner portals and monthly statement runs, screening/leasing integrations (applications, e-sign), utility-billing modules, smart-building/access systems at managed properties, and the PM's own office IT. Multi-property staff work across sites and remotely.

## Two data populations and the scope boundary

- **Owner vs tenant separation:** the platform holds owner financials (statements, draws, reserves) and tenant PII (SSNs from applications, payment details, lease terms) — distinct audiences that must never cross. Portal-permission mistakes that expose one owner's data to another, or owner data to tenants, are serious incidents: capture facts, flag the PM's principal/compliance owner. Never bulk-change portal permissions without the PM's sign-off.
- **The support boundary:** tenants and owners are the PM's customers, not the MSP's users. The desk supports the PM's staff and systems; tenant "I can't log into the portal" issues route per the PM's documented process (usually the PM's staff or the platform vendor's tenant support). Know where that line is documented and don't quietly absorb tenant support — but during a genuine portal outage, give the PM staff clear status they can relay.

## Money adjacency — trust accounting and fraud

- PM firms hold owner and tenant funds in regulated trust/escrow accounts (state real-estate rules). Never modify accounting configuration, payment routing, bank details, or disbursement settings — those are the PM's licensed responsibility with the platform vendor; the desk's role is facts and facilitation.
- Rent-payment redirect fraud is this vertical's BEC flavor: spoofed "pay rent to this new account" emails to tenants, or compromised staff mailboxes ahead of owner draws. Any changed-payment-details report or mailbox-compromise suspicion → run security/vendor-fraud-bec-alert immediately, principal notified, evidence preserved.

## The rhythm and what's sacred

- **Rent week:** the 1st through ~5th of the month is peak load — portal payments, late-fee processing, and staff workload all spike. No discretionary changes to the platform path, portal, or payment integrations from month-end through the 5th. A portal-payment failure on the 1st is top severity.
- **Owner-statement and draw runs:** monthly, typically mid-month — accounting-module problems then have owner-facing consequences the PM cares about intensely.
- **Leasing season:** student housing and many markets turn over in summer — mass move-in/move-out weeks stress screening, e-sign, and onboarding flows.
- **Sacred:** the tenant-payment path in rent week, the maintenance-request chain (urgent habitability requests must flow), trust-accounting integrity, and owner/tenant data separation.

## Steps

1. Context: search_tickets for this PM's history with the named platform, and search_itglue / search_hudu for the platform flavor and hosting, portal and payment-processor details, maintenance-chain integrations, the tenant-support boundary, and the PM's approver for permission/config changes.
2. Fraud screen when applicable: changed payment details, tenants reporting odd payment emails, or staff-mailbox oddities → security/vendor-fraud-bec-alert path immediately, before routine troubleshooting.
3. Set severity on the PM clock: portal-payment failure in rent week or a broken maintenance chain (urgent requests stranded) → top severity; single-staff or single-workstation issues → normal. Ask "is this affecting tenants/owners right now, and where are we in the month?"
4. Split the failure domain: PM-staff-side (workstation, office network, identity) vs platform/vendor-side vs payment-processor-side vs a specific integration. Vendor status pages early; the honest "it's vendor-side, case opened, here's what staff can tell tenants" beats local thrashing.
5. Run the LOB framework for platform issues: version/plan, change correlation, verbatim error, scope (one property? one portfolio? everyone?), vendor-escalation package with case number. Permission or configuration changes execute only with the PM's documented approver's sign-off.
6. Note in plain text: system, PM-clock context (rent week / statement run), scope, error verbatim (tenant/owner PII scrubbed — unit numbers over names), boundary handoffs, vendor case, and verification — a staff member processes a real payment/work order end to end, or the vendor confirms restoration.

## Guardrails

- Never modify trust-accounting configuration, payment routing, bank details, or disbursement settings — facts to the platform vendor and the PM principal only; the desk is not in the funds path.
- Changed-payment-details reports and mailbox-compromise suspicions are incidents → security/vendor-fraud-bec-alert, principal notified, evidence preserved.
- Owner/tenant data separation is sacred: no bulk portal-permission changes without PM sign-off; suspected cross-exposure → facts only, flag the principal/compliance owner.
- Respect the documented tenant-support boundary — route tenant issues per the PM's process rather than absorbing them, while equipping staff with honest status during outages.
- Rent week (month-end through the 5th) is a change-freeze window for the platform, portal, and payment paths.
- Minimum-necessary PII: unit/property identifiers over tenant names; no application data (SSNs, screening results) in tickets ever.
- Docs tools vary per tenant — state what you could not verify; an undocumented tenant-support boundary or missing platform-approver contact is a flag worth raising.
