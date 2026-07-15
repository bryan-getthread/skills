---
name: Linear Fix-Shipped Loop
description: Check escalated Linear issues for ones that reached Done, then comment back on every linked ticket so technicians verify the fix and close — closing the loop engineering escalations usually leak.
category: Escalation
tools: [list_issues, get_issue, list_comments, list_issue_statuses, search_tickets, add_ticket_note, update_ticket]
---

# Linear Fix-Shipped Loop

The other half of the engineering-escalation bridge: when an escalated defect is fixed and shipped, every ticket that was waiting on it gets told — with a verify-and-close prompt — instead of aging silently in a waiting status.

## When to use

- "Check whether any of our engineering escalations shipped" / "any Linear fixes we can close tickets on?"
- Run as a periodic sweep (e.g. daily) over issues created by the Engineering Escalation to Linear skill.
- A technician asks "did the fix for <issue-id> ship? Can I close my tickets?"

## Steps

1. Confirm the Linear connector is available (member-authenticated; if absent, stop and say so).
2. Gather the watchlist: Linear issues escalated from the desk — by the agreed label (e.g. "from-service-desk"), or the specific issue IDs the member provides. Resolve status meanings via list_issue_statuses rather than assuming "Done" is the terminal name.
3. For each issue in a completed status, read get_issue and list_comments for the fix description, the release/version it shipped in, and the ticket references recorded when it was escalated.
4. Find the linked tickets: the references on the Linear issue, plus a search_tickets pass for the escalation note stamp ("Escalated to engineering as <issue-id>") to catch tickets linked after filing. Skip tickets already closed.
5. For each open linked ticket, post a plain-text note: the fix shipped, in which version/release, what the technician should verify with <client>, and a prompt to close the ticket once verified — or to reply on the Linear issue if the problem persists. Do not close tickets yourself.
6. Optionally (with confirmation) move linked tickets from a vendor/engineering-waiting status back to an active status via update_ticket so they resurface in techs' queues.
7. Report: issues shipped, tickets annotated per issue, tickets skipped (already closed), and any issue whose ticket links were missing — flag those for manual reconciliation.

## Guardrails

- Never close tickets on the strength of a Done status — a human verifies with the client first. The note prompts verification; it does not declare the client's problem solved.
- Only annotate tickets actually linked to the issue by reference; never fan a fix note out on symptom similarity.
- Read the release state honestly: "merged" is not "deployed" — if the issue or comments don't say it shipped to production, say the fix is pending release rather than shipped.
- Each ticket gets the note once — check for an existing fix-shipped note before posting (safe to re-run).
- Notes are plain text; requires member-connected Linear.
