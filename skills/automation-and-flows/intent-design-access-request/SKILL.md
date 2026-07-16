---
name: Access Request Intent Design
description: Build the access-request intent for folders, distribution lists, and shared mailboxes — capturing resource, justification, and approver so the ticket arrives approvable. Use when asked to "build an access request intent", "automate permission requests", or when access changes rank high in Intent Mining.
category: Automation & Flows
tools: [list_intents, get_intent, create_intent, update_intent, set_variation_arguments, set_variation_replies, update_variation, search_tickets]
---

# Access Request Intent Design

Guide the admin through building an access-request intent that turns "can I get access to the finance folder" into a ticket carrying the exact resource, the access level, a business justification, and the named approver — so the technician's first action is granting, not chasing.

## When to use

- "Build an intent for folder/DL/mailbox access requests."
- "Access tickets never say what access or who approved it."
- Intent Mining flagged permission/access changes as a top request type.

## Design

**Trigger phrases (adapt to real ticket language):** "need access to", "can I get access", "add me to the distribution list", "share the folder with me", "permission to the shared drive", "access to the shared mailbox", "can't open the folder", "add me to the group", "need to see the <department> drive", "request access".
Near-miss watch: "can't open the folder" may be a permissions *problem* rather than a request — the intent should ask which; "add a new user" belongs to the new-hire intent.

**Arguments to collect:**
- Who needs access (requester themselves, or on behalf of <user>).
- The exact resource: folder path / DL name / shared mailbox / application role, and the platform if ambiguous.
- Access level: read vs. edit/full; send-as vs. send-on-behalf for mailboxes.
- Business justification (one sentence — required, it is what the approver reads).
- Named approver: resource owner or requester's manager per client policy.
- Duration: permanent or time-boxed (project end date).

**Reply flow:**
1. Collect arguments; confirm the summary including the exact resource string.
2. If the client's policy says the approver must sign off first → reply that the request was logged and approval is being sought; ticket carries the approver for the desk's approval step (send_approval on the desk side pairs well here).
3. Create the ticket with a plain-text field block; route to the access/security board the client uses.
4. Never grant, promise, or imply access in the reply — "your request has been submitted", not "you'll have access shortly".

**Handoff rule:** all grants are human/workflow actions after approval. Requests for admin/privileged roles, security groups, or another user's mailbox contents get flagged elevated-risk and are never treated as routine.

**Variation hooks (per client):** approver policy (resource owner vs. manager vs. central IT contact), platforms in play, whether time-boxed access is the default, elevated-risk resource list.

**Success metric:** first-touch completeness — percentage of access tickets arriving with resource + level + justification + approver filled; secondary, median time from request to grant. Deflection is minimal by design; the win is intake quality.

## Steps

1. `list_intents` — check for an existing access/permissions intent; prefer updating.
2. `search_tickets` for recent access requests; mine phrasing for triggers and the back-and-forth for missing-argument candidates; note which resources recur (variation seed).
3. Draft the full spec with the elevated-risk flag rule explicit. Test plan: 5 should-match, 3–5 should-not (include a "can't open file — corrupted" troubleshooting near-miss and a new-hire near-miss).
4. Show the draft; on explicit confirmation call `create_intent` then the variation tools.
5. Report what was created, restate the test plan, recommend activation after tests pass. Do not activate.

## Guardrails

- Intent tools are admin-only; if absent, output the complete written spec for an admin to apply.
- The intent never grants or promises access; replies must not state or imply approval outcomes.
- Privileged/admin access and access to another person's data are always flagged elevated-risk, never routine intake.
- Do not invent the client's approval policy or resource names; placeholder (<folder>, <distribution list>) and flag before activation.
- Confirm before any write; never activate on the member's behalf. Ticket field block in plain text.
