---
name: Notion QBR Page
description: Assemble a QBR pre-read for a client as a Notion page — ticket volume vs prior period, top issues, SLA picture, and recommendations, with data views where a database backs them. Use when asked to "prep my QBR in Notion", "build the pre-read for <client>'s business review", or "QBR page for next week".
category: Connectors
tools: [search_tickets, search_clients, notion-search, notion-create-pages, notion-update-page, notion-create-view, add_ticket_note]
---

# Notion QBR Page

Build the document the CSM walks into the QBR with: real numbers from the ticket record, the quarter's story in three sections, and recommendations that follow from the data — as a shareable Notion page.

## When to use

- "I have a QBR with <client> — build the pre-read in Notion."
- "Assemble <client>'s quarterly numbers into a page I can present from."
- Recurring quarterly prep across the CSM's book of business.

## Steps

1. Confirm the client (`search_clients`) and period (default: last full quarter vs the quarter before).
2. Gather the data with `search_tickets`, split per signal: total volume, volume by category/board, open vs closed, aged tickets, escalations, and recurring issues (same user/device/symptom 3+ times). If any search hits its cap, carry "at least N" into the page — never present a capped count as exact.
3. Draft the page structure: **The quarter in numbers** (volume vs prior period, resolution picture), **What drove the tickets** (top 3–5 themes with counts; noisy users/devices as patterns, e.g. "<device> generated 12 tickets"), **Where we stand** (open items, risks), **Recommendations** (2–3, each tied to a data point above — hardware refresh, training, automation, monitoring change).
4. `notion-search` for the teamspace/section where QBR docs live (or where last quarter's page sits) and confirm placement; then `notion-create-pages`. If a QBR tracking database exists, add the page there and optionally `notion-create-view` (table or chart) over it for the trend view.
5. Offer a follow-up: `add_ticket_note` links or a summary back to any open tickets referenced as QBR commitments.

## Guardrails

- **Member-authenticated connector:** requires the member's Notion connection; degrade by delivering the full pre-read as chat/markdown for manual paste, and say how to connect Notion.
- Every number on the page comes from an actual search result; recommendations without a supporting data point don't ship. No invented benchmarks.
- This page may be shown to the client: no internal-only commentary (tech performance gripes, margin data), no other clients' names, no end-user blame by name — describe patterns, not people.
- Result-cap honesty carries into the page text itself, not just the chat.
- Update-don't-duplicate: if this quarter's page already exists, `notion-update-page` it.
