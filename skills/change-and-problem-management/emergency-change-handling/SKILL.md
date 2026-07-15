---
name: Emergency Change Handling
description: A change had to happen NOW to restore or protect service — run the break-glass discipline: minimal in-flight record, act-then-document is allowed, but the full retro-documentation is mandatory within 24 hours and this skill chases it to done.
category: Change & Problem Management
tools: [search_tickets, create_ticket, update_ticket, add_ticket_note, send_approval, list_boards]
---

# Emergency Change Handling

Emergency changes trade approval order for speed — they never trade away the record. The deal is explicit: you may act before the paperwork, and in exchange the paperwork is not optional, not partial, and not late. This skill covers both halves: the minimal record during the emergency, and the retro-documentation debt after it.

## When to use

- "We had to change the firewall rule at 2am to stop the bleeding — paper it."
- An incident response is about to make a production change with no time for normal approval.
- The morning-after sweep: find last night's emergency changes and verify their retro-documentation.
- An emergency change flagged by change-request-prerequisites as missing retroactive documentation.

## Steps

**During (or immediately after) the emergency:**

1. Verify it qualifies: an active incident or imminent harm where waiting for normal approval is more damaging than acting. Name the incident ticket. "Urgent for the client" without service impact is a normal change on a fast calendar — route it back.
2. Create the emergency change record (create_ticket on the change board, title "EMERGENCY CHANGE: <system> — <one-line what>") with the minimum viable in-flight fields: what is being changed, why now (link the incident), who is executing, and the intended reversal if it makes things worse. Thirty seconds of typing, not a CAB submission.
3. Capture the authorization actually obtained — even emergencies need a named human saying go (on-call lead, client emergency contact per the desk's policy). Record who authorized and how (send_approval where the desk uses it; a note naming the verbal authorizer and time otherwise). If the executor authorized themselves, record that fact plainly — it's reviewable, not hidden.
4. As actions happen, timestamp them in notes as close to real time as possible. Contemporaneous fragments beat a polished reconstruction.

**Within 24 hours (the mandatory retro):**

5. Complete the record to full change-request standard: final what/why/scope, actual actions taken with times, actual result, rollback status, post-change validation evidence, and the risk assessment written honestly after the fact (what the blast radius really was). Incomplete after 24h → escalate to the change owner/lead by note; the debt does not silently age out.
6. Route the completed record for retroactive review: the approver (or next CAB via cab-brief-builder, which lists all emergency changes) reviews whether the emergency call was right and whether the change stays or gets re-done properly.
7. If the emergency fix is a temporary patch rather than the real fix, open the follow-up normal change now and link it — emergency fixes have a habit of becoming permanent by inertia.

## Steps (morning-after sweep variant)

1. search_tickets for emergency changes created in the last 24–48h; for each, check retro-documentation completeness against step 5's list; post an itemized gap note on incomplete ones and flag repeat offenders (same tech, serial incomplete retros) to the lead.

## Guardrails

- Emergency is a justification class, not a convenience class — no incident, no emergency. The agent pushes back on laundering normal changes through the fast lane.
- Act-then-document never becomes act-then-nothing: the 24h retro is the hard rule this skill exists to enforce.
- The agent papers and chases; it does not execute the change and does not retroactively approve it — the retro review is a human's call.
- Never backfill the record to look like approval preceded action when it didn't — the honest sequence, timestamped, is the defensible one.
- Plain-text notes throughout; this trail is exactly what an auditor or an angry client will read later.
