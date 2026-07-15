---
name: Supporting Nonprofits
description: Vertical pack for nonprofit clients — donor systems (Blackbaud-class), grant/donated licensing (TechSoup, Microsoft nonprofit grants), board-and-volunteer access hygiene, budget sensitivity, and the year-end giving season. Load when the client is a nonprofit or the ticket names a donor CRM, donated licenses, or volunteer access.
category: Industry Packs
tools: [search_tickets, search_knowledge_base, search_itglue, search_hudu, add_ticket_note, update_ticket, web_search]
---

# Supporting Nonprofits

A nonprofit runs on trust and margin: donor data it must never leak, licenses it gets at charity rates it must never lapse, and a revolving door of volunteers and board members who all "just need access for a bit." This pack layers nonprofit-vertical knowledge onto the LOB Application Framework (troubleshooting-playbooks/lob-application-framework).

## When to use

- The client is a nonprofit, association, or foundation, or the ticket names Blackbaud (Raiser's Edge NXT, Financial Edge), DonorPerfect, Bloomerang, Little Green Light, Neon CRM, Salesforce Nonprofit Cloud, or a donation/giving platform
- License questions involving TechSoup, Microsoft/Google nonprofit grants, or donated/discounted subscriptions
- Access requests for volunteers, board members, or seasonal staff
- Anything scheduled near year-end giving season (November–December)

## The stack you'll meet

- **Donor CRM is the crown jewels:** Blackbaud Raiser's Edge NXT at the established end; DonorPerfect, Bloomerang, Neon CRM, Little Green Light in smaller orgs; Salesforce Nonprofit Cloud where there was a techie board member. It holds donor identities, giving history, and payment methods — a leak here is an existential reputation event for the org.
- **Giving/payments:** online donation platforms and embedded payment processors. Payment flows are PCI territory — the desk configures around them per vendor documentation and never handles or stores card data itself.
- **Finance:** Financial Edge, QuickBooks (often the nonprofit edition), and grant-tracking spreadsheets that are load-bearing. Fund accounting means their books are audited against restricted funds — finance-system tickets near an audit get the same care as tax season at a CPA firm.
- **Licensing is its own subsystem:** Microsoft 365 nonprofit grants and discounts, Google for Nonprofits, TechSoup-brokered donations, and vendor charity tiers. Eligibility is periodically revalidated; grant licenses have caps and mixing (some free, some discounted) is normal. A "we suddenly lost licenses" ticket is often an eligibility-revalidation lapse, not a technical failure — check the grant/eligibility status first.

## Access hygiene: the volunteer and board problem

- **Churn is the norm.** Volunteers, interns, seasonal campaign staff, and board members rotate constantly, often using personal email addresses and personal devices. The standing risks: orphaned accounts with donor-data access, shared "volunteer@" logins, and board members who left two years ago still in the finance system.
- The desk's posture: every access grant gets an expiry or review date recorded; shared accounts are flagged and pushed toward named accounts (or at minimum documented and MFA'd); offboarding a volunteer is a real offboarding, not a shrug. Propose a quarterly access review to the org's operations owner — small orgs rarely think of it until the audit or the incident.
- **Board members are high-privilege outsiders:** they see finances and strategy but are outside the org's device management. Prefer least-privilege portal/share access over adding personal devices to the tenant; flag exceptions to the org's leadership rather than deciding unilaterally.

## Budget sensitivity and the rhythm

- **Every recommendation has a price tag question.** Check nonprofit/charity pricing before quoting anything — most major vendors have a tier, and proposing full commercial pricing to a nonprofit erodes trust. When a fix has a free-tier path and a paid path, present both honestly.
- **Giving season (November–December, peaking Giving Tuesday and December 29–31):** the donation platform, website, and donor CRM are sacred. No discretionary changes to any giving-path system from mid-November through January 2 without explicit sign-off; a donation-page outage on December 31 costs the org real, unrecoverable revenue.
- **Other rhythm points:** the annual gala/event (registration and AV become temporarily critical), grant-application deadlines, and the fiscal-year-end audit window (finance system stability + auditor access requests, which follow the same approval discipline as any access grant).

## What's sacred

Donor data confidentiality above all; the giving path during giving season; grant/donated license standing (a lapsed eligibility or misused grant license can cost the org its discount program — flag anything that looks like out-of-policy license use to the org's owner rather than papering over it); and the finance system during audit.

## Steps

1. Context: search_tickets for this org + app history, and search_itglue / search_hudu for the stack documentation: donor CRM, licensing sources (grant vs paid, TechSoup records), giving-platform inventory, and the org's designated approver for access and spending.
2. Triage by mission impact and calendar: giving-path outage in season → top severity; donor-data exposure suspicion → incident handling and flag the org's leadership/compliance owner immediately; single-user issues → normal flow with cost-free workarounds preferred.
3. For license tickets: determine the license's *source* (grant, TechSoup donation, charity discount, paid) before touching it — check eligibility/revalidation status for sudden losses, and never "fix" a shortage by assigning licenses outside the grant's permitted users; escalate genuine shortfalls to the org with the nonprofit-pricing options laid out.
4. For access tickets: apply the hygiene posture — named accounts, least privilege, recorded expiry/review date, org-approver sign-off for anything touching donor or finance data. Offboardings get the full treatment (sessions, MFA, shared-credential rotation where documented).
5. For app failures, run the LOB framework: exact versions/plan tier, change correlation (donor CRMs are cloud-heavy — check vendor status pages early), verbatim error, known-issue search, and vendor-escalation package through the org's support entitlement when it's vendor territory.
6. Note in plain text, donor-data-scrubbed: system, scope, season context, license source if relevant, approvals obtained, error verbatim, branch, vendor case, verification by the user running the real workflow (record a test gift per vendor procedure, pull a report).

## Guardrails

- Donor data gets PHI-grade ticket hygiene: no donor lists, giving amounts, or payment details in tickets or screenshots; minimum necessary always; suspected exposure → facts + flag org leadership. No legal advice on donor-privacy or fundraising regulations.
- Never handle, store, or transcribe card data; payment-flow work follows the vendor's documented procedures only.
- Never assign or repurpose grant/donated licenses outside their permitted scope — program eligibility is at stake; flag out-of-policy use to the org's owner.
- Giving-season change freeze on the donation path (mid-November–January 2) without explicit sign-off.
- Every access grant for volunteers/board carries an expiry or review date in the ticket; shared accounts are flagged, not created.
- Cost consciousness is a guardrail: check nonprofit pricing before recommending spend, and say when a free path exists.
- Docs tools vary per tenant — state what you could not verify; an org with no recorded license sources or access approver is itself a flag worth raising.
