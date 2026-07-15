---
name: ISP Outage Tracking
description: Manage the lifecycle of a circuit outage that is in the carrier's hands — capture the carrier ticket reference, run the escalation clock, and keep the client informed on a cadence while waiting on the provider. Use when a site's internet/WAN is down and the ISP owns the fix.
category: Devices & Infrastructure
tools: [search_itglue, search_hudu, search_tickets, update_ticket, add_ticket_note, schedule_ticket, web_search, send_approval]
---

# ISP Outage Tracking

Own the waiting: once an outage is confirmed to be the carrier's problem, this skill structures the carrier ticket reference, escalation timeline, and client communication cadence so "waiting on the ISP" never means "nobody is watching".

## When to use

- A site's internet or WAN circuit is down and troubleshooting points at the carrier.
- A carrier ticket exists but the Thread ticket has gone quiet ("what's the status with the ISP?").
- Deciding when to escalate a carrier ticket that has blown past its ETR.
- A multi-site client wants updates while a circuit is out.

## Steps

1. Confirm the handoff is justified before parking the ticket on the carrier: demarc-side evidence (modem/ONT lights, carrier status page via `web_search`, other tenants at the site affected) should be noted in the ticket. If the CPE (firewall/router) has not been ruled out, route back to `network-outage-triage` first.
2. Capture the carrier engagement as structured data in a plain-text note (`add_ticket_note`): carrier name, carrier ticket/reference number, circuit ID (from `search_itglue` / `search_hudu` circuit documentation — reference IDs only, never account passwords/PINs beyond noting where they are documented), date/time opened, ETR given, name of the carrier rep if provided, and the callback commitment.
3. Set the ticket state honestly: `update_ticket` to the waiting-on-vendor status the board uses, with the carrier reference in the summary so anyone picking it up sees it instantly.
4. Run the escalation clock: schedule a follow-up (`schedule_ticket`) at the earlier of the carrier's ETR or the desk's standard check interval (commonly 2–4 hours for a hard-down circuit; follow the client's SLA if documented). At each check: call/portal-check the carrier, append the update with a timestamp, and if the ETR has slipped twice or the carrier has gone unresponsive past its own commitment, escalate — ask for a supervisor/escalation desk and record the new reference. Business-impacting outages with no ETR after the first escalation may justify invoking the account team or a formal escalation email; get the service-desk lead's sign-off (`send_approval`) before invoking contractual remedies.
5. Client communication cadence, independent of carrier progress: an update at engagement, at every ETR change, and at a fixed interval even when there is nothing new ("still with the carrier, next update by <time>"). "No news" updates are sent, not skipped. Match the client's documented communication preferences.
6. On restoration: verify from the site side (not just the carrier's word) before closing — connectivity restored, VPN/tunnels re-established, no flapping for a stabilization period. Close with a final note containing the full carrier timeline, and if the outage may qualify for an SLA credit, flag that to whoever handles the client's billing rather than promising a credit yourself.

## Guardrails

- Never invent or approximate a carrier ticket number, circuit ID, or ETR. If the reference is missing, the first action is obtaining it.
- Do not promise restoration times to the client beyond what the carrier stated, and attribute the estimate ("the carrier's current ETR is...").
- Credentials, account PINs, and portal passwords for carrier accounts stay in documentation — never in ticket notes.
- Waiting-on-vendor is a monitored state: a carrier ticket with no scheduled follow-up is a workflow failure. Always leave the next check booked.
- SLA-credit eligibility is flagged, never asserted — contract interpretation belongs to account management.
- Notes are plain text — no markdown, no emojis.

## See also

- `network-outage-triage` — the diagnosis phase that precedes this skill.
- `circuit-inventory` — where circuit IDs and carrier references should already live.
- `vendor-escalation-package` (escalation) — building a formal escalation dossier when the carrier stalls.
