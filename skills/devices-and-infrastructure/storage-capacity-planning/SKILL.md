---
name: Storage Capacity Planning
description: Turn repeated disk-space alerts into a trend-based capacity forecast per server/NAS — growth rate, projected full date, and an expansion-options brief the account team can price. Use when a device alerts on disk for the third time, or someone asks "how long until this fills up" or "what should we buy".
category: Devices & Infrastructure
tools: [search_ninjaone_devices, get_ninjaone_device, get_ninjaone_device_activities, list_ninjaone_alerts, search_tickets, search_itglue, search_hudu, add_ticket_note, create_ticket]
---

# Storage Capacity Planning

Stop treating the same disk alert as a new incident: reconstruct the growth trend from alert and ticket history, project when the volume actually fills, and produce an options brief — clean up, expand, or re-architect — with enough substance for a client conversation.

## When to use

- The same server or NAS has alerted on disk space more than once in recent months.
- "How long until <server>'s data drive is full?"
- Cleanup bought some space last time and the alert is back — the growth is organic.
- The account team wants an expansion recommendation to take to the client.

## Steps

1. Establish current state: `get_ninjaone_device` for present capacity and free space per volume; documentation (`search_itglue` / `search_hudu`) for what the volume holds (file shares, databases, backup targets — the workload determines both growth character and expansion options).
2. Reconstruct the trend from history, since the RMM MCP exposes point-in-time readings: harvest prior data points from `list_ninjaone_alerts` and `get_ninjaone_device_activities` (each threshold alert is a dated reading) and `search_tickets` for earlier disk tickets that recorded free-space figures. Three or more dated points make a usable trend; fewer means the honest output is "insufficient history — start recording, re-forecast in N weeks" plus the current-state brief.
3. Compute growth honestly: rate per month from the points you have, projected-full date at the current rate, and a sensitivity line ("at 1.5x the rate, full by <earlier date>"). Deduct known one-off events (a migration, a cleanup) from the trend rather than letting them distort it — say when you did. State the forecast as a range, not a date certain.
4. Separate reclaimable from organic growth: from ticket history and workload knowledge — old user profiles, stale snapshots, log bloat, duplicate backup copies are reclaimable (route quick wins via `disk-space-remediation`); steady line-of-business data growth is organic and cleanup only rents time. Quantify roughly how much runway cleanup buys versus the growth rate.
5. Expansion-options brief, matched to the platform from documentation: extend the volume/datastore (if the underlying storage has headroom), add disks/shelf to the server or NAS (check slot availability and warranty/EOL status — cross-reference `hardware-refresh-forecast` for boxes near end of life, where expanding a dying unit is money misspent), tier or archive cold data, or move the workload (larger NAS, SAN, cloud). For each: rough effort class, disruption (window needed?), and how much runway it buys at the observed rate. No vendor pricing from memory — sizing and prices are for the account team to quote.
6. Output: current state, trend with data points shown, projected-full range, reclaimable-vs-organic split, and the options table. Post the plain-text summary via `add_ticket_note`, and open a `create_ticket` for the account team when the projected-full date is inside the client's typical procurement lead time — that timing gap is the whole point of forecasting.

## Guardrails

- Never present a projected-full date as certain — always a range with the data points that produced it visible, so a human can judge the trend quality.
- A forecast from fewer than three dated readings is labeled as low-confidence or not issued at all; do not dress up two points as a trend.
- Cleanup recommendations name what the files are believed to be and require verification before deletion — nothing is deleted by this skill.
- No hardware model or price quotes from memory; the options brief describes classes of solution and defers sizing/pricing to quoting.
- Do not forecast backup-target volumes as if they were organic data growth — retention settings, not users, drive them; say when a volume's growth is policy-driven.
- Notes are plain text — no markdown, no emojis.

## See also

- `disk-space-remediation` — the immediate cleanup pass; this skill answers what happens after cleanup.
- `hardware-refresh-forecast` — whether the box is worth expanding.
- `hypervisor-alerts` — datastore-specific capacity emergencies.
