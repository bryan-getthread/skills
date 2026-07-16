---
name: Liongard QuickBooks Online Read
description: Interrogate a client's QuickBooks Online company through the Liongard QuickBooks Online inspector — users and roles/access, admins, connected apps, company/subscription info. Use for "who has access to their QBO?", access/admin reviews, or offboarding checks during triage. Read-only; financial data is out of scope.
category: Liongard Inspectors
tools: [liongard_environment, liongard_launchpoint, liongard_metric, liongard_query, liongard_detection, liongard_timeline, search_tickets, search_itglue, search_hudu, add_ticket_note]
---

# Liongard QuickBooks Online Read

Reads QuickBooks Online company state — users and their roles/access level, admins, connected apps, and company/subscription info — from the Liongard QBO inspector's dataprint, so a tech can run access reviews on a sensitive financial system without logging into QBO. This skill reads *access and configuration*, not financial records.

## When to use

- "Who has access to <client>'s QuickBooks Online?" — access review or offboarding.
- "Who is the primary/company admin?" — admin review of a sensitive system.
- "What apps are connected to their QBO?" — third-party-access/exposure check.
- "What subscription/company info is on file?"
- Post-change: "did a user's access level or a connected app change?"

## What the inspector captures

Users with role/access level, admin/primary-admin designation, connected third-party apps, and company/subscription metadata. Exact coverage varies by inspector version. Financial transactions/ledger data are NOT the target of this skill.

## Steps

1. Follow liongard-inspectors/liongard-access-pattern: resolve the environment, `liongard_launchpoint` filtered by systemType "QuickBooks Online" / "QuickBooks", verify last-run success, note dataprint age.
2. Pick the angle and query via `liongard_metric` or `liongard_query`. Illustrative JMESPath angles — verify field paths against the live dataprint; schemas vary by inspector version:
   - Users: user list with role/access level and status.
   - Admins: primary/company admin(s).
   - Connected apps: third-party app authorizations.
   - Company info: subscription/company metadata.
3. For "what changed" questions, pull `liongard_detection` / `liongard_timeline` scoped to QBO: user/access-level changes, admin changes, connected-app adds — line them up against the incident window.
4. Sanity-check surprising values (zero users, no admin) — usually a wrong field path or partial inspection. Re-probe broadly.
5. Output: the requested facts in a compact table, source + dataprint age line, and flags (ex-employee still has access, excess full-access users, unexpected connected apps). Offer a plain-text `add_ticket_note`.

## How this feeds tickets

- **Offboarding**: confirm a departing user's QBO access — feeds the offboarding checklist. Findings are inputs, not the removal action.
- **Security review**: who has access to finance systems and what apps are connected are high-value audit findings — export with their as-of date.

## Guardrails

- Read-only: this skill never changes QBO users, access levels, or connected apps. Remediation is the client/tech's job in QBO. Never convert a finding into a completed action.
- Financial data is out of scope — do not read, summarize, or note ledger/transaction data even if the dataprint exposes it. Access and config only.
- QuickBooks Online is a sensitive financial system — keep notes to access/config facts, no financial figures.
- Always state dataprint age; access state changes — an old dataprint on an active offboarding deserves a "verify live" caveat.
- If no QuickBooks Online launchpoint exists, degrade per the access pattern: docs → ticket history → "verify with the client".
- Access findings are review inputs, not accusations — "unrecognized user, verify" phrasing.
- Plain-text notes only.
