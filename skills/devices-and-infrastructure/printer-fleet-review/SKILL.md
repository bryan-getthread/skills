---
name: Printer Fleet Review
description: Cluster a client's printer-related tickets to find the chronic devices, quantify the time they burn, and recommend replace vs repair per device. Use for "why do we get so many printer tickets from <client>" or a print-environment health pass.
category: Devices & Infrastructure
tools: [search_tickets, search_itglue, search_ninjaone_devices, add_ticket_note]
---

# Printer Fleet Review

Mines the client's ticket history for printer pain, attributes it to specific devices, and turns "printers are always broken" into a per-device verdict: fix the spooler pattern, replace the chronic unit, or leave it alone.

## When to use

- "<client> complains about printers constantly — what's actually going on?"
- QBR prep wants the printer story with numbers behind it.
- Deciding whether a specific troublesome printer is worth another repair.
- Recurring-issue mining flagged printing as a top category for a client.

## Steps

1. Pull the client's printer-related tickets via `search_tickets` — search per signal rather than one broad query: printing/printer terms, spooler, common vendor names in the client's environment, and known printer hostnames. Window: 90–180 days. Flag result caps and report "at least N" where a search may be truncated.
2. Attribute each ticket to a device where possible: printer names/IPs in ticket text, cross-referenced against documentation (`search_itglue` printer/asset records) and any print servers or networked printers visible in the RMM (`search_ninjaone_devices`). Tickets that name no device go into an "unattributed" bucket — report its size honestly rather than distributing it by guesswork.
3. Cluster and count: tickets per device, per failure type (driver/spooler, connectivity, hardware jams/quality, supplies, user-how-to), and per user or location. Separate infrastructure patterns (one print server's spooler dying weekly — a server problem, not a printer problem) from device patterns (one unit jamming monthly).
4. Compute the cost signal: ticket count x rough handling time per ticket (from time entries where present; otherwise say estimates are used). A device generating hours of tech time per quarter has a comparable monthly cost to a lease on its replacement — make that comparison explicit but label the numbers as estimates.
5. Verdict per chronic device (3+ tickets in the window): repair/adjust when failures share one fixable cause (driver standardization, firmware, network drop); replace when failures are hardware-diverse on an aging unit, repair costs approach replacement, or supplies/parts are discontinued; investigate when the pattern points at environment (network segment, print server) rather than the unit.
6. Output: summary (printer tickets as share of the client's volume, trend vs prior period), chronic-device table (device, tickets, failure types, hours, verdict), infrastructure findings, and the unattributed count. Recommend next actions per verdict. Offer a plain-text note via `add_ticket_note`.

## Guardrails

- Replace-vs-repair is a recommendation with the evidence shown — never a purchase commitment, and never with invented pricing; hardware costs are the requester's to fill in.
- Attribution honesty: do not smear unattributed tickets across devices to make clusters look stronger.
- Time-cost figures built on estimates are labeled as estimates.
- Result-cap honesty across every ticket search; split searches per signal.
- Client-facing versions of this review go through the Client-Facing Device Report skill's sanitization rules — this output is internal.
- Plain-text notes only.
