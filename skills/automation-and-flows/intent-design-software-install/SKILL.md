---
name: Software Install Intent Design
description: Build the software-request intent — approved-list check, license implications, and the approval path designed in so installs stop arriving as bare "please install X". Use when asked to "build a software install intent", "automate app requests", or when software installs rank high in Intent Mining.
category: Automation & Flows
tools: [list_intents, get_intent, create_intent, update_intent, set_variation_arguments, set_variation_replies, update_variation, search_tickets, search_knowledge_base]
---

# Software Install Intent Design

Guide the admin through building a software-request intent that checks the ask against the client's approved-software list, surfaces license cost implications, and routes through the right approval path — producing a ticket that is either instantly actionable or already in front of the person who can say yes.

## When to use

- "Build an intent for software install requests."
- "Users ask for random apps and techs don't know what's approved."
- Intent Mining flagged application installs/licenses as recurring volume.

## Design

**Trigger phrases (adapt to real ticket language):** "install software", "need <application> installed", "can I get <application>", "install an app on my computer", "need a license for", "software request", "download and install", "can you add <application> to my machine", "need access to <application>" (when it means install, not permissions), "new program".
Near-miss watch: "<application> not working" is troubleshooting; "need access to <application>" where the app is already installed is an access request; "update <application>" may be patching.

**Arguments to collect:**
- Which application (exact name/vendor; version if it matters) and for whom.
- Business purpose (one sentence — the approver reads this).
- Paid or free/open-source, if the user knows — the desk verifies regardless.
- Needed-by date.
- Manager/approver if the client requires per-request sign-off.

**Reply flow (approved-list gate):**
1. Check the ask against the client's approved-software list (maintained in the KB or the variation config).
2. **On the approved list, no license cost** → reply that it is approved software; create an install ticket routed for standard fulfillment.
3. **On the list but licensed/paid** → reply that a license is involved and approval for the cost is needed; ticket carries the approver and the license note.
4. **Not on the list / unknown** → reply that the app needs a security-and-licensing review first; ticket routed to the client's software-approval path, flagged "not on approved list". No wording that implies it will be approved.
5. Never provide download links or walk the user through self-installing — installation is a desk action under the client's endpoint policy.

**Handoff rule:** every install is executed by the desk/tooling, never by chat instructions. Unknown software is a security decision, not a fulfillment task.

**Variation hooks (per client):** the approved-software list (or KB article that holds it), license-owner contact, whether users have local-install rights at all, security-review routing.

**Success metric:** percentage of software tickets arriving pre-classified (approved / license-needed / review-needed) with purpose and approver attached; secondary, reduction in installs performed for unapproved software.

## Steps

1. `list_intents` — check for an existing software-request intent; prefer updating.
2. `search_tickets` for recent install/license requests; mine phrasing and see which apps recur (approved-list seed and variation content).
3. `search_knowledge_base` for an approved-software article per client; if none exists, flag that the list itself is a prerequisite and must come from the admin — do not invent one.
4. Draft the full spec: triggers, arguments, the three-branch gate, variations. Test plan: 5 should-match, 3–5 should-not (app-broken and permissions near-misses).
5. Show the draft; on explicit confirmation call `create_intent` then the variation tools.
6. Report what was created, restate the test plan, recommend activation after tests pass. Do not activate.

## Guardrails

- Intent tools are admin-only; if absent, output the complete written spec for an admin to apply.
- The approved-software list must come from the client/admin — never fabricate approval status; unknown means the review branch.
- No download links, no self-install walkthroughs, no license keys in replies — ever.
- Replies must not promise approval, cost coverage, or install timelines.
- Confirm before any write; never activate on the member's behalf. Field block in plain text.
