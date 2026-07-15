---
name: Liongard AWS Read
description: Answer AWS account questions from the Liongard AWS inspector — IAM users and access-key age, root/MFA posture, S3 exposure flags, security groups, and resource census — without console access.
category: Liongard Inspectors
tools: [liongard_launchpoint, liongard_metric, liongard_detection, liongard_timeline, liongard_query, search_tickets, add_ticket_note]
---

# Liongard AWS Read

Interrogate a client's AWS account through the Liongard AWS inspector. AWS at MSP clients is usually a handful of workloads set up years ago and rarely reviewed — which makes the dataprint's IAM and exposure sections disproportionately valuable: old access keys, users without MFA, and public S3 buckets are exactly what nobody is watching.

## When to use

- "Who has IAM access to <client>'s AWS?" / "any access keys older than 90 days?"
- "Are any of <client>'s S3 buckets public?" / exposure sweep before a security review
- "What's actually running in their AWS account?" — census for scoping, cost review, or offboarding a vendor
- AWS change context on an incident ("did a security group open up recently?")

## Steps

1. Base pattern (skills/liongard-inspectors/liongard-access-pattern/SKILL.md): liongard_launchpoint filtered by the AWS systemType for the client. Verify the inspector exists and last ran successfully; date the dataprint. Note the inspected account/region scope — multi-account clients may have one inspector on one account.
2. Route the question:
   - IAM hygiene → liongard_metric over IAM users: console vs. programmatic access, MFA enabled, access-key age and last-used, attached policies. Angle shapes: `Users[?MfaEnabled==\`false\`]`, `Users[].AccessKeys[?Age > \`90\`]` — verify field paths against the live dataprint. Root-account MFA status, if captured, is the first line of the report regardless of the question.
   - Every IAM user gets the ownership test: keys belong to a person or a documented service. Cross-check unrecognized users against search_tickets and docs; an unclaimed IAM user with active keys is treated like an unexplained admin (security/global-admin-audit discipline) and escalates if recent.
   - S3 exposure → buckets flagged public or with permissive ACLs/policies where the dataprint captures them. Public buckets are findings, not stats — flag with bucket purpose unknown/known, route remediation to a technician.
   - Network exposure → security groups allowing 0.0.0.0/0 on management or database ports.
   - Census → EC2 instances (type, state), RDS, Lambda, load balancers as captured. Stopped-but-provisioned and oversized resources are cost-review candidates — configuration signals only, never dollar figures.
   - Changes → liongard_detection / liongard_timeline for IAM changes, security-group modifications, new resources in an incident window.
   - Open-ended → liongard_query, verified with targeted metrics before anything actionable is reported.
3. Output: direct answer, evidence rows, dataprint as-of timestamp, inspected-scope statement. Plain-text ticket note on request.

## Guardrails

- Scope honesty: the inspector sees one account (and possibly limited regions); "none found" always carries "in inspected scope."
- Key age is risk, not proof of abuse — recommend rotation through the client's change process; never present rotation as done.
- Root-account and public-bucket findings lead the output whenever present.
- Read-only: no IAM, bucket-policy, or security-group changes originate here.
- Degradation: no AWS inspector → ticket history and docs only, labeled "not instrumented"; if the client has meaningful AWS and no inspector, that gap is itself a recommendation.
