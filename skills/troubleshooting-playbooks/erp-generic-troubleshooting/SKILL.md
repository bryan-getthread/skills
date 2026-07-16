---
name: ERP Generic Troubleshooting
description: Work any ERP ticket (NetSuite, Dynamics, Epicor, Acumatica, or a vertical ERP) by isolating the failing tier — client, app server, database, web, or integration job — while respecting vendor-managed databases and month-end change freezes.
category: Troubleshooting Playbooks
tools: [search_tickets, search_knowledge_base, search_itglue, search_hudu, add_ticket_note, web_search]
---

# ERP Generic Troubleshooting

ERPs differ in vendor and vocabulary but not in shape: a client tier, an application tier, a database the vendor usually controls, and a web of integration jobs feeding it. The desk's job is rarely to fix the ERP — it is to isolate which tier failed, gather the evidence, and route it to the right owner (internal infra, the ERP vendor, or the integration owner) without touching anything the support contract says not to.

## When to use

- "The ERP is down / slow / throwing errors" for a client-server or web ERP the desk doesn't specialize in
- One module or one user fails while the rest of the system works
- A nightly sync, EDI feed, or integration job into the ERP stopped landing
- Any ERP ticket during month-end or year-end close

## Steps

1. **History first.** search_tickets for this client + the ERP name. Prior tickets reveal who the real owner is (vendor case history, a named integration partner) and what "normal" looks like.
2. **Docs second.** search_itglue / search_hudu / search_knowledge_base for the ERP profile: product and version, hosted where (on-prem, vendor cloud, third-party host), who holds the support contract, which tiers the MSP is allowed to touch, and any documented integration jobs.
3. **Check the freeze.** Ask whether the client is in month-end/year-end close or has a documented change-freeze window. During close: diagnose freely, change nothing without the client's finance owner explicitly accepting the risk, and say which fixes are deferred and why.
4. **Identify versions and topology.** Client version vs server version, and whether the failure is in a thick client, a web front end, or both. Never assume the architecture — a "web ERP" often still has an on-prem integration agent.
5. **Evidence before theory.** Exact error text, the ERP's own log (client log vs server log — get whichever matches the failing tier), timestamp of first failure, and what changed (updates, network work, credential rotations).
6. **Branch by tier:**
   1. **Client tier** — one user or one machine. Compare against a working peer: same version? same config file/connection profile? Fails on the user's account on another machine (profile/account) or any account on this machine (endpoint)? Escalate when: the thick client needs a version-matched reinstall governed by the vendor's compatibility matrix — verify the matrix via web_search or the vendor portal before touching it.
   2. **Application tier** — everyone fails, database reachable. Check service state, disk, memory, recent patching on the app server (guidance to the tech; no execution). Escalate when: app-tier services crash-loop or the server is vendor-hosted — open the vendor case with logs attached rather than restarting things blind.
   3. **Database tier** — vendor-managed in most contracts. The desk's role is evidence only: capture the DB error the app reports, note timing. Do NOT run queries, kill sessions, or "quick fix" a vendor-managed database — a well-meant change here can void support. For stuck locks, use the LOB Database Locks playbook (vendor-approved methods only). Escalate when: any database-layer fault — to the DB owner named in docs, with the evidence package.
   4. **Web tier** — browser-side errors on a web ERP. Isolate with a clean browser profile and a second browser (see the Browser Issues playbook); if TLS/certificate errors appear, check the SSL Inspection Issues playbook before blaming the ERP. Escalate when: the vendor's status page confirms a platform incident — link it and stop local troubleshooting.
   5. **Integration jobs** — data stopped flowing but the ERP itself is fine. Identify the job from docs (EDI, PSA sync, warehouse feed), find its last-success timestamp and its error output, and check the usual suspects: expired credential/API key, a changed endpoint, a full disk on the middleware host. Escalate when: the integration is owned by a third party — package last-success time, error text, and record counts for them; do not re-run jobs that post financial transactions without the client's say-so, since re-runs can double-post.
7. **Close the loop.** Verify with the user in the failing workflow — not just a login. Post a plain-text note: tier isolated, evidence, owner routed to, what was changed (or deliberately not changed, and why), verification result.

## Guardrails

- No script or remote execution — all remediation is guidance or a documented handoff; use the RMM deep link for hands-on work when that integration is enabled.
- Vendor-managed database means hands off: evidence in, escalation out. Never kill DB sessions or run repair tooling ad hoc.
- Never re-run financially significant integration jobs without explicit client approval — duplicate postings are worse than late ones.
- Month-end freeze is real: default to diagnose-only during close windows unless the client's finance owner accepts the change.
- Build vendor escalations with the LOB Application Framework playbook's package standard: version, change history, exact errors, logs, scope.
- Do not invent compatibility rules or KB links — web_search and cite. search_itglue / search_hudu may be absent for this tenant; fall back to search_knowledge_base and say what you could not check.
- Notes destined for a PSA sync: plain text, no markdown or emojis.
