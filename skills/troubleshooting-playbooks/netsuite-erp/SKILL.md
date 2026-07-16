---
name: NetSuite ERP
description: Support NetSuite tickets safely as an MSP — a cloud ERP where the fixes are roles/permissions, saved-search/report visibility, and integration (SuiteScript/REST/CSV) errors, and where the account is the client's system of record — from the exact error and role context, never by editing financial config. Escalate customization to the NetSuite admin/partner.
category: Troubleshooting Playbooks
tools: [search_tickets, search_knowledge_base, search_itglue, search_hudu, add_ticket_note, web_search]
---

# NetSuite ERP

NetSuite is a cloud, client-server-less ERP — there is no server to fix and no file to repair. Almost every MSP-supportable NetSuite ticket is really about **access** (roles/permissions), **visibility** (a saved search or report), or an **integration** feeding data in/out. This playbook works those, and draws a hard line at financial configuration and customization, which belong to the client's NetSuite administrator or implementation partner.

## When to use

- "I can't see / access <record, transaction, report>" or a permission/"insufficient privileges" error
- A saved search or report returns wrong/no results, or a user can't run one they should
- An integration (CSV import, SuiteScript, a REST/SOAP/third-party connector) is failing or throwing errors
- A user can't log in, has the wrong role, or landed in the wrong subsidiary/center

## Steps

1. **Scope the MSP boundary first.** search_itglue / search_hudu / search_knowledge_base for the NetSuite footprint: who administers it (in-house admin vs a NetSuite implementation partner — MSP support is usually access/integration plumbing, not ERP config), the roles model, subsidiaries/OneWorld if present, and any integrations the MSP actually owns. Establish what is *yours to touch* vs the admin's/partner's — financial setup, workflows, and customizations are theirs.
2. **History first.** search_tickets for this client + NetSuite: a recent role change, a NetSuite release upgrade (NetSuite pushes two major releases a year — behaviour and deprecations shift), an integration change, or a new user/subsidiary. A group of users losing access at once usually traces to a role edit.
3. **Get the exact error and role context before theorizing.** NetSuite errors are specific — the verbatim message, and critically **which role the user was in** (permissions are role-scoped, and a user with multiple roles behaves differently per role). For integrations, the execution log / script deployment log or the import error rows. Read the actual error and role — permissions problems are meaningless without the role.
4. **Branch:**
   1. **Roles / permissions** — "insufficient privileges" or a missing record/tab: this is a role permission or a record-level restriction, not a bug. Identify the role and the specific permission/level it lacks. Changing a role's permissions affects everyone in that role and can expose financial data — this is the NetSuite admin's decision; gather the evidence (role, record, permission needed) and route it, don't self-edit roles. Escalate when: the fix is a role/permission change — admin owns it.
   2. **Saved search / report** — wrong or empty results: usually the search's own criteria/filters, the running user's role/subsidiary restricting visible rows, or available-vs-audience settings on the saved search. Read the search definition and the user's role context. A report that "broke" often had its criteria or a referenced field changed. Fixing a shared saved search affects everyone who uses it — confirm ownership before editing.
   3. **Integration errors** (CSV / SuiteScript / REST/SOAP / connector) — read the log: a CSV import failing on specific rows (field mapping, mandatory fields, reference lookups), a SuiteScript error in the execution log, or a REST/token-based-auth failure (expired token/role, a release change deprecating an endpoint). MSP-owned integration plumbing (auth, connectivity, mapping) is fair game to fix; SuiteScript **code** is the developer's/partner's. Escalate when: the failure is in custom script logic.
   4. **Login / wrong role or center** — can't sign in or lands in the wrong place: check the user's login status, assigned roles, and default role/center; SSO/2FA problems pair with the identity playbooks. A user "missing everything" is often just defaulted into a limited role — switching role solves it.
5. **Verify and note.** Success is the user completing the real action in the correct role (sees the record, runs the search, the integration processes cleanly). Plain-text note: the boundary (what was MSP vs admin), exact error, role context, branch, action or handoff, verification.

## Guardrails

- **NetSuite is the client's financial system of record** — never edit financial configuration, workflows, GL/accounting setup, or customizations; those belong to the NetSuite admin or implementation partner. MSP scope is access/visibility/integration plumbing. When unsure, escalate rather than change.
- Role/permission changes affect everyone in the role and can expose financial data — package the evidence and route to the admin; do not self-edit roles to "unblock" a user.
- No mass data operations — never bulk-edit/delete records or mass-run an import to "fix" data without the admin's sign-off; NetSuite mass updates are irreversible at scale.
- There is no server/file to fix — don't chase infrastructure causes; the fixes live in roles, searches, and integrations.
- SuiteScript/custom-code errors are the developer's/partner's — diagnose and hand off, don't edit code.
- Do not invent permission names, log locations, or release behaviours — web_search NetSuite's current docs and cite; behaviour shifts each release.
- Docs coverage varies per tenant — note what you couldn't check. Notes destined for a PSA sync: plain text, no markdown or emojis.
