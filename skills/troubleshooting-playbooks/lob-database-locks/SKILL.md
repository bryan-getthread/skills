---
name: LOB Database Locks
description: Clear "record is locked by another user" and stuck-session tickets in line-of-business apps — identify the locking session through the app's own admin tooling and release it by the vendor-approved method only; never kill database sessions ad hoc.
category: Troubleshooting Playbooks
tools: [search_tickets, search_knowledge_base, search_itglue, search_hudu, add_ticket_note, web_search]
---

# LOB Database Locks

Someone opened a record, went to lunch, and now billing can't post. The lock ticket is a race between the fast wrong fix (kill the session at the database) and the slow right one (find the session and release it the way the vendor says to). This playbook enforces the second — because a session killed mid-write is how a lock ticket becomes a data-corruption ticket.

## When to use

- "Record locked by another user" and that user is gone, at lunch, or "definitely not in it"
- The whole LOB app says a batch/period/module is locked and nobody can work
- A stuck login: the user crashed out and the app still counts them as signed in (sometimes eating a license)
- Repeat lock tickets on the same app — the pattern, not just today's lock

## Steps

1. **History first.** search_tickets for this client + the app + lock symptoms. The prior lock ticket usually documents the vendor-approved release method — the single most valuable artifact for this playbook. Repeat tickets shift the goal from clearing today's lock to fixing the cause.
2. **Docs second.** search_itglue / search_hudu / search_knowledge_base for the app's profile: vendor, version, database engine underneath, whether the database is vendor-managed, and — the key item — the documented session/lock management procedure. If none is documented, finding it (vendor KB via web_search, or asking the vendor) is step one, and documenting it afterward is part of closing the ticket.
3. **Identify versions.** App and version; lock-management procedures are version-specific, and old advice against a new version is how damage happens.
4. **Evidence before theory.** The exact lock message (record vs batch vs module vs login lock — different mechanisms), which user/session the app names as the holder, and when the lock appeared. Then verify reality: is that user actually in the app? Ask them or their neighbor before assuming a ghost session.
5. **Branch:**
   1. **The holder is real and present** — not a stuck lock at all. The record is legitimately in use; coordinate between users. Escalate when: never — this is a conversation, not a ticket escalation.
   2. **Ghost session, app-level release exists** — the user crashed/disconnected and the app has an admin console for sessions (most LOB apps do: a user/session manager, a "clear locks" or "release user" function). Confirm with the named user that they are truly out and have no unsaved work, then guide the app admin through the vendor's release procedure. This is the standard path — the app's own tooling releases cleanly because it knows its transaction state. Escalate when: the console shows the session but release fails — vendor support with the session details; do not drop to the database layer on your own.
   3. **No app-level tool, or lock survives it** — the vendor's documented method may involve a lock file to remove (some file-based LOB apps) or a vendor utility. Follow only a written vendor procedure for this exact version (web_search the vendor KB; cite it in the ticket). Preconditions: all users out of the app where the procedure requires it, and a current backup confirmed before touching lock files. Escalate when: the only remaining option is killing a session at the database engine — STOP. That is vendor territory: get vendor support on the case, and act only under their direction. If the database is vendor-managed, it was never the desk's to touch (see ERP Generic Troubleshooting).
   4. **License-eating stuck logins** — users blocked because ghost sessions consume seats. Same discipline as branch 2, plus note the frequency: if crashes keep abandoning sessions, the crash is the real ticket (pair with the LOB Application Framework playbook to root-cause and build the vendor case).
   5. **Recurring locks** — same record type, same time of day, same workflow. Look for the pattern: a report that opens tables exclusively during business hours, a user habit (record left open overnight), a client/app timeout setting the vendor allows tuning. Recommend the pattern fix to the client and vendor; clearing lock after lock is treadmill work.
6. **Close the loop.** Have the blocked user retry the exact operation, and check the app for error flags on the affected record (a released lock with a half-written record is not a win — if the app offers a data-verify function, recommend running it). Post a plain-text note: lock type, holder session, method used and its source (vendor KB reference), who confirmed the user was out, verification result.

## Guardrails

- Never kill database sessions ad hoc — no KILL SPID, no engine-level session drops, no deleting lock files outside a written, version-matched vendor procedure. The lock is annoying; a torn transaction is a restore.
- Confirm the human is out first: releasing a session with unsaved work destroys that work. Get an explicit "I'm out, nothing unsaved" from the named user or their manager, and note who said it.
- Backup before any lock-file or utility-based clearing; if backup state is unknown, that is a stop condition.
- Vendor-managed database = evidence and escalation only; the desk does not connect to it.
- No script or remote execution — every procedure is guidance for the app admin or tech, or a vendor case; do not fabricate vendor procedures or KB references — web_search and cite the real one.
- search_itglue / search_hudu may be absent for this tenant — fall back to search_knowledge_base and say what you could not check.
- Notes destined for a PSA sync: plain text, no markdown or emojis.
