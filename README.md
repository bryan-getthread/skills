# Thread Skills

A library of ready-to-use Skills for the Thread agent (Super Magic). Each Skill is a small, reusable instruction set that tells the agent how to run one MSP workflow well: triaging a ticket, drafting a client reply, prepping a QBR, responding to a phishing report.

These are drawn from how partners actually use the agent. Where a workflow showed up many times in slightly different forms, we merged the variants into one canonical Skill that covers the superset of what they did. Nothing here contains partner names, client data, or environment-specific detail. Read a Skill, adapt the specifics to your stack, and load it.

## How a Skill works

A Skill is a folder with a `SKILL.md`. The frontmatter carries the metadata the agent needs; the body is the workflow.

```
---
name: Ticket Triage
description: When to reach for this skill, in one line.
category: Triage & Routing
tools: [search_tickets, list_boards, list_ticket_statuses]
---

# Ticket Triage
...steps, guardrails...
```

- **name** shows in the Skills picker.
- **description** is the trigger. Write it as *when to use this*, because that is what the agent matches against.
- **tools** lists the tools the Skill expects. Names follow Thread's tool set (`search_tickets`, `add_ticket_note`, `update_ticket`, and so on) plus any integration tools like the NinjaOne or IT Glue families.
- The body is the instruction set: a short setup, numbered steps, and guardrails.

## Using a Skill

Copy the body of a `SKILL.md` into a new Skill in Thread, or paste it into the agent directly. Adjust the specifics that are yours: board names, status names, tone rules, and which integrations you run. The guardrails are the part to keep. They are what stops the agent from bulk-closing tickets or pasting a password into a ticket body.

## Catalog

### Triage & Routing
- **[Ticket Triage](skills/triage-and-routing/ticket-triage/SKILL.md)** — classify, gauge severity, catch duplicates, recommend where it goes.
- **[Catchall Routing](skills/triage-and-routing/catchall-routing/SKILL.md)** — find the real client and contact for a no-company ticket and reassign it.
- **[Dispatch & Workload](skills/triage-and-routing/dispatch-and-workload/SKILL.md)** — balance the queue by load and produce a morning dispatch view.

### QA & Closure
- **[Ticket QA Review](skills/qa-and-closure/ticket-qa-review/SKILL.md)** — check a completed ticket against your closure standard, pass or bounce it.
- **[Aging, SLA & Follow-Up](skills/qa-and-closure/aging-and-sla-cleanup/SKILL.md)** — surface stale, SLA-risk, and waiting-on-client tickets and drive them to resolution.

### Communication
- **[Client Reply](skills/communication/client-reply/SKILL.md)** — draft an external reply in your house voice and format.

### Documentation
- **[Ticket Summary & Closure Note](skills/documentation/ticket-summary-and-closure-note/SKILL.md)** — summarize what happened, formatted for a resolution note.
- **[Time Entry Cleanup](skills/documentation/time-entry-cleanup/SKILL.md)** — turn rough time notes into standardized entries and a work summary.
- **[KB Article Draft](skills/documentation/kb-article-draft/SKILL.md)** — turn a resolved ticket into a reusable KB or SOP draft.
- **[Ticket Intake & Formatting](skills/documentation/ticket-intake-formatting/SKILL.md)** — build a clean title and description from a raw report.

### Escalation
- **[Escalation Prep](skills/escalation/escalation-prep/SKILL.md)** — build a full escalation package so the next tier can pick it up cold.
- **[Shift & Call Handoff](skills/escalation/shift-handoff/SKILL.md)** — a one-minute handoff of open work and blockers.

### Onboarding & Access
- **[New Hire Onboarding](skills/onboarding-and-access/new-hire-onboarding/SKILL.md)** — account, licenses, access, hardware, MFA.
- **[Employee Offboarding](skills/onboarding-and-access/employee-offboarding/SKILL.md)** — securely disable a leaver and reclaim access and assets.
- **[Password & MFA Recovery](skills/onboarding-and-access/password-and-mfa-recovery/SKILL.md)** — reset safely with identity checks and a secure handoff.

### Devices & Infrastructure
- **[Device Health Check](skills/devices-and-infrastructure/device-health-check/SKILL.md)** — RMM diagnostics for a device or a client's fleet.

### Security
- **[Phishing Triage](skills/security/phishing-triage/SKILL.md)** — assess a reported email, contain it, reply with a verdict.
- **[Security Alert Response](skills/security/security-alert-response/SKILL.md)** — triage SOC, breached-credential, and dark-web alerts.

### Reporting & Account Management
- **[Client Health Report](skills/reporting-and-account-management/client-health-report/SKILL.md)** — a period read on a client's support health.
- **[QBR & SBR Prep](skills/reporting-and-account-management/qbr-and-sbr-prep/SKILL.md)** — internal prep brief for a business review.
- **[One-on-One Prep](skills/reporting-and-account-management/one-on-one-prep/SKILL.md)** — a candid brief on a technician's recent work.
- **[My Queue Summary](skills/reporting-and-account-management/my-queue-summary/SKILL.md)** — an individual tech's open tickets with a clear next step.

## Contributing

New Skills and improvements are welcome. See [CONTRIBUTING.md](CONTRIBUTING.md). The bar: a Skill solves one real workflow, carries guardrails for anything destructive, and contains no client or partner data.
