---
name: Supporting Architecture and Engineering Firms
description: Vertical pack for architecture, engineering, and surveying clients — CAD/BIM stacks (AutoCAD, Revit, Civil 3D-class), network license servers, workshared central models and file locking, the GPU workstation fleet with certified drivers, and submittal-deadline urgency. Load when the client is an AEC firm or the ticket names CAD/BIM software, a license server, plotters, or drawing files.
category: Industry Packs
tools: [search_tickets, search_knowledge_base, search_itglue, search_hudu, add_ticket_note, update_ticket, web_search]
---

# Supporting Architecture and Engineering Firms

An AEC firm bills hours drawn in a handful of heavyweight applications, against permit and submittal deadlines that do not move. The recurring failure surfaces are licensing (a license server or subscription hiccup idles the whole studio), the central-model/file-locking layer where multiple people edit one building, and workstations whose GPUs and drivers must match vendor-certified combinations. This pack layers AEC knowledge onto the LOB Application Framework (troubleshooting-playbooks/lob-application-framework).

## When to use

- The client is an architecture, engineering (civil/structural/MEP), surveying, or design-build firm, or the ticket names AutoCAD, Revit, Civil 3D, MicroStation, SketchUp, Rhino, ArchiCAD, Bluebeam, Navisworks, or BIM 360/Autodesk Construction Cloud
- "No licenses available," license-server or subscription sign-in failures
- "The central model is corrupt," "someone has the file locked," sync/worksharing errors, Xref path breakage
- Workstation performance, GPU/driver, or new-CAD-machine spec tickets; plotter/large-format printing issues
- Anything due against a submittal or permit deadline

## The stack you'll meet

- **CAD/BIM applications:** AutoCAD and its verticals (Civil 3D, MEP), Revit for BIM, MicroStation in transportation/utilities work, SketchUp/Rhino for design, Bluebeam for markups, Navisworks for coordination. Version discipline matters intensely: Revit files upgrade one-way (a file saved in a newer version cannot be opened by an older one), and project teams — including external consultants — pin a version per project. Never upgrade a user's Revit/CAD version ad hoc; the project's version is a team decision.
- **Licensing:** modern Autodesk is named-user subscription (sign-in problems, seat assignment); legacy and other vendors still run network license servers (FlexLM-class) — a stopped license service or expired license file idles everyone at once, making the license server a P1 single point of failure. Know which model each product uses at this firm.
- **Central models and file locking:** Revit worksharing puts a central model on a file server or in BIM 360/ACC cloud worksharing, with users syncing local copies. Classic tickets: stale locks after a crash, sync conflicts, "central model corrupt" (often recoverable via vendor-documented audit/recreate procedures — vendor path, not improvisation), and Xref/path breakage after file-server changes. Any file-server rename, migration, or permission change at an AEC firm must account for drawing references and central models first.
- **Hardware:** GPU workstations with vendor-certified driver requirements (check the application vendor's certified-hardware/driver list before updating — the newest driver is frequently wrong), dual/large displays, SpaceMouse-class peripherals, and large-format plotters whose driver quirks generate steady tickets.

## The rhythm and what's sacred

- **Deadline-driven everything:** submittal, permit, and bid deadlines are contractual. Ask "what's due and when?" on every ticket — a plot failure the afternoon of a submittal is top severity regardless of technical size.
- **Coordination cadence:** model-exchange days with external consultants (weekly uploads/downloads) create predictable file-transfer and version-mismatch tickets.
- **Sacred:** the project file server and its backup (drawings are the firm's product and legal record — retention matters for liability), central models, and the license path. Overnight is when backups, renders, and point-cloud processing run — schedule reboots around them.

## Steps

1. Context: search_tickets for this firm's history with the named product (stale-lock clears and plotter fixes repeat), and search_itglue / search_hudu for versions per project where documented, license model and server details, central-model locations, backup scope, and the plotter fleet.
2. Triage on the deadline clock: firm-wide license or file-server failure, or a failure blocking a stated submittal → top severity; single-user, single-file issue → normal with a workaround (another workstation, a recovered local copy) stated honestly.
3. License tickets: identify the model first (named-user sign-in vs network license server). Server model → check the license service, license-file expiry, and port reachability; named-user → seat assignment and identity path. State plainly when it's the vendor's licensing outage.
4. Central-model/file tickets: never edit or "repair" a central model ad hoc — follow the vendor-documented recovery sequence (audit, recreate central from a good local, restore from backup) and preserve copies before every step. Stale locks get cleared per vendor procedure after confirming the holder truly crashed out.
5. Run the LOB framework for app failures: exact versions and build/update levels, GPU driver against the certified list, change correlation (Windows/driver updates breaking viewport rendering is the classic), verbatim error, vendor-escalation package with case number.
6. For file-server changes (migrations, renames, permissions): inventory Xref paths and central-model references first, plan the repath, and schedule outside deadline windows with the firm's BIM/CAD manager (or the closest equivalent) signed off.
7. Note in plain text: product + exact versions, project-deadline context, scope, error verbatim, recovery steps with copies preserved, vendor case, verification — the user opens, syncs, and plots the real project.

## Guardrails

- Never upgrade CAD/BIM versions on a user's machine without the project team's version decision — a one-way file upgrade can lock external consultants out of the model.
- Never perform destructive central-model surgery outside vendor-documented procedure; preserve copies before every recovery step.
- GPU drivers on CAD workstations follow the vendor's certified list, not "latest" — document the driver version installed.
- File-server changes at AEC firms are planned changes with reference-path inventory first, never quick fixes.
- Drawing retention is a liability matter — never purge old project files or adjust retention without the principal's direction.
- Respect the deadline freeze: no discretionary changes to license servers, file servers, or plot paths when a submittal is imminent.
- Docs tools vary per tenant — state what you could not verify; undocumented license servers or central-model locations are flags worth raising.
