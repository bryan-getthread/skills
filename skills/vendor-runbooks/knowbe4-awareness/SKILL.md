---
name: KnowBe4 Awareness & PhishER
description: Coordinate a KnowBe4 program — security-awareness training campaigns and phishing simulations — and triage user-reported emails flowing in through PhishER / the Phish Alert Button, keeping simulation reports from colliding with real-phish investigation. Verify against KnowBe4's current documentation.
category: Vendor Runbooks
tools: [search_tickets, search_clients, search_contacts, search_knowledge_base, add_ticket_note, create_ticket, schedule_ticket]
---

# KnowBe4 Awareness & PhishER

Vendor specialization for KnowBe4's two connected surfaces: **KMSAT** (Security Awareness Training + phishing simulations) and **PhishER** (the triage queue fed by user-reported emails via the Phish Alert Button). phishing-simulation-program owns the campaign canon and security-awareness-coordination owns training follow-up; this skill adds KnowBe4's mechanics and the PhishER↔real-phish interplay. Verify product/console names against KnowBe4's current documentation.

## When to use

- A client is starting/running KnowBe4 training or phishing simulations and the desk is coordinating scope, cadence, and delivery whitelisting
- User-reported emails are arriving through PhishER / the Phish Alert Button and need triage
- Simulation results or training-completion data need a readout and next-cycle plan

## Steps

1. Program coordination → follow phishing-simulation-program for scope (all staff, ramping by department is fine — exempting executives is not), cadence, difficulty progression, and the no-shame culture rule. KnowBe4-specific: confirm the simulation sending/link domains are documented in the client's knowledge record (search_knowledge_base) and whitelisted at the mail gateway so results measure users, not the spam filter — create a coordination ticket (create_ticket) for the gateway work and schedule the campaign window (schedule_ticket).
2. Simulation ↔ real interplay — the coordination step that protects the desk: the phishing-triage simulation branch matches against the documented KnowBe4 simulator domains. If those domains aren't recorded, every simulation email becomes a real investigation and every user reply skews the client's metrics. A user reporting a simulation is a GOOD outcome — closed internally without replying. Real phishing never pauses for a campaign: anything not matching the documented simulator domains gets full real-phish treatment. Never assume "it's probably the simulation."
3. PhishER triage — user-reported email via the Phish Alert Button lands in PhishER:
   - Simulation match → close as simulation (report = success), no reply.
   - Real suspicious mail → run phishing-triage: verify headers/sender, check for sibling deliveries to other users, and if credentials may have been entered or a payload run, branch to compromised-account-containment / edr-detection-runbook. PhishER's disposition and any tenant-wide message pull (PhishRIP-style search-and-remove) are technician actions in the console — the agent scopes the search and records it, but does not claim a removal it can't perform.
   - Distinguish the report volume: a spike of the same lure across many users is a live campaign, not noise — treat one representative as the investigation and the rest as siblings.
4. Readouts: report cohort-level metrics — report rate (the headline metric to grow), click rate, credential-entry rate, training completion — with result-cap honesty on any ticket-derived counts; named-individual results go only to the client's designated program owner, never into general reporting. Feed repeat-risk patterns to security-awareness-coordination for targeted training.

## Guardrails

- No-shame is non-negotiable: named-individual results go only to the program owner; general reporting stays cohort-level — no "wall of shame."
- Only an exact match against documented simulator domains closes a report as simulation; partial matches get investigated as real, always.
- Don't send or schedule simulation emails yourself and don't claim a PhishER quarantine/removal — those are console actions the platform owner/technician executes; the agent coordinates and records.
- Simulator domains and scope belong in the client's documentation before the first send — refuse to mark a campaign "ready" without that record.
- Real phishing found via PhishER gets the full investigation canon — identity first if a login/credential is involved, contain fast, document the decision.
- Degradation: without search_knowledge_base, confirm the simulator-domain record with the tech directly and note where it lives.
