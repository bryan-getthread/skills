# Skill authoring spec

The format, quality bar, and capability ground rules for every skill in this library.
Written for humans and for AI writers generating skills at scale.

## File format

One folder per skill: `skills/<category-slug>/<skill-slug>/SKILL.md`

```markdown
---
name: Short Title Case Name
description: When to reach for this skill, in one line — it is the trigger the agent matches.
category: Human Readable Category
tools: [tool_a, tool_b]
---

# Short Title Case Name

One- or two-sentence framing: what this skill does and the outcome it produces.

## When to use

- 2–5 bullets of concrete situations (mirror real prompts partners type).

## Steps

1. Numbered, imperative steps. Reference tools by name where a call happens.
2. Keep branches inline ("If X → do Y; otherwise → Z").
3. End with what to output/post and in what format.

## Guardrails

- The non-negotiables. Confidence gates before writes. What NOT to do.
- Degradation note when an integration may be absent for the tenant.

## Unattended (Flows) variant   ← only for skills that run embedded in Flows

- Output discipline ("your entire reply is posted verbatim — no narration").
- Deterministic stop conditions; when in doubt, do nothing.
```

## Quality bar (from what actually propagates)

1. **Description = trigger.** Write it as *when to use this*; the agent matches against it.
2. **One workflow per skill.** Superset of the partner variants, not ten near-duplicates.
3. **Guardrails are the product.** Every skill that writes must carry: confidence gate,
   confirm-before-destructive, and a "when in doubt, do nothing" posture where relevant.
4. **Result-cap honesty.** If a search may hit a cap, say so in output rather than
   presenting an estimate as exact. Split searches per signal per board when scanning.
5. **Plain-text notes** where PSA compatibility matters (no markdown/emojis in notes
   destined for a PSA sync).
6. **No fabrication.** "Do not invent links, ticket numbers, or documentation."
   Zero-assumption rule: never convert a recommendation into a completed action.
7. **Sanitized.** No partner/client names, no people, no credentials, no board/person IDs.
   Use placeholders: <client>, <user>, <device>, "the VIP board".
8. **Localizable.** No idioms that break translation; note where desks run non-English.

## Capability ground rules (July 2026 — see research/tool-surface.md)

**Thread native (always safe to reference):** search_tickets, create_ticket, update_ticket,
add_ticket_note, log_time_entry, merge_ticket, schedule_ticket, update_schedule_entry,
assign_contact, send_approval, run_assistive_ai, search_clients, search_contacts,
search_members, search_knowledge_base, search_thread_docs, web_search, list_boards,
list_ticket_statuses, list_ticket_priorities, list_recap_templates,
create/update/get/list_flow(+actions/filter_attributes/filter_attribute_values),
create/update/get/list_intent, set_variation_arguments, set_variation_replies,
update_variation, create_timezest_scheduling_request, get_timezest_scheduling_requests,
list_timezest_appointment_types, list_timezest_resources.

**In-app SuperAgent only (note degradation for external MCP):** runbooks family
(list_ticket_runbooks, recommend_runbooks, get_runbook_form_link, search_runbooks,
open_runbook_form), views family (view_list, view_save, view_duplicate,
view_listFilterAttributes, view_searchFilterValues, view_getCurrent), skills family
(load_skill, create_skill, update_skill, list_skills, run_skill_on_ticket),
view_openDraft, send_client_email, create_recap.

**NinjaOne (when enabled):** list_ninjaone_organizations, search_ninjaone_devices,
get_ninjaone_device, get_ninjaone_device_activities, get_ninjaone_device_link,
list_ninjaone_alerts, reset_ninjaone_alert, list_ninjaone_windows_services,
control_ninjaone_windows_service, reboot_ninjaone_device, set_ninjaone_device_maintenance,
set_ninjaone_device_approval.
⚠️ NOT supported: running scripts, deploying software, pushing policies, script library.
The remediation handoff is get_ninjaone_device_link (deep link the tech in).

**Liongard (when enabled):** liongard_environment, liongard_launchpoint, liongard_alert,
liongard_detection, liongard_metric, liongard_events, liongard_timeline, liongard_device,
liongard_identity, liongard_domain, liongard_cyber_risk_dashboard, liongard_report,
liongard_agents, liongard_query.

**Docs (when enabled):** search_itglue, search_hudu.
**ConnectWise RMM (when enabled):** connectwise_rmm_search_devices,
connectwise_rmm_list_companies, connectwise_rmm_get_device (+ patch/software reads).
**ImmyBot (when enabled):** search_immybot_computers, list_immybot_tenants,
get_immybot_computer, list_immybot_maintenance_sessions (read-only today).

**MCP connectors (member-authenticated; note in guardrails that the member must have
connected them):**
- Notion: notion-search, notion-fetch, notion-create-pages, notion-update-page,
  notion-move-pages, notion-duplicate-page, notion-create-database,
  notion-update-data-source, notion-create-view, notion-update-view,
  notion-query-data-sources, notion-create-comment, notion-get-comments, notion-get-users.
- Linear: list_issues, get_issue, create_issue, update_issue, list_comments,
  create_comment, list_issue_statuses, list_issue_labels, create_issue_label,
  list_projects, create_project, update_project, list_cycles, list_teams, list_users,
  list_documents, get_document.
- Zapier: reference as `zapier: <App> "<Action>"` (e.g. `zapier: Microsoft Teams "Send
  Channel Message"`). Verified inventories in research/connectors.md. Known gaps:
  NO Microsoft Bookings, NO Planner, Teams has no meeting-create (use Outlook "Create
  Event"), Entra ID has writes but no user search.

## Category taxonomy

triage-and-routing · qa-and-closure · communication · documentation · escalation ·
onboarding-and-access · devices-and-infrastructure · security · reporting-and-analytics ·
account-management · scheduling-and-dispatch · automation-and-flows · finance-and-billing ·
sales-and-quoting · client-lifecycle · training-and-enablement · personal-productivity ·
troubleshooting-playbooks · compliance-and-audit · connectors (cross-tool workflows that
exist because of Notion/Linear/Zapier)
