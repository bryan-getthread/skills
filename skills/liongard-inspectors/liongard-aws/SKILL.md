---
name: Liongard AWS Read
description: Answer AWS account questions from the Liongard AWS inspector — IAM users and access-key age, root/MFA posture, S3 exposure flags, security groups, and resource census — without console access.
category: Liongard Inspectors
tools: [liongard_launchpoint, liongard_metric, liongard_detection, liongard_timeline, liongard_query]
connectors: [Liongard]
---

# Liongard AWS Read

**When to use:** "Who has IAM access to <client>'s AWS / any access keys older than 90 days?", "are any S3 buckets public?", an AWS resource census for scoping or offboarding, or AWS change context on an incident.

## Prompt

```
Answer an AWS question for CLIENT_NAME from the Liongard AWS inspector's dataprint. Read-only — no IAM, bucket-policy, or security-group changes originate here.

1. liongard_launchpoint filtered by the AWS systemType. Verify the inspector exists and last ran successfully; date the dataprint. Note the inspected account/region scope — multi-account clients may have one inspector on one account, so "none found" always carries "in inspected scope."
2. Route the question, verifying every JMESPath field path against the live dataprint:
   - IAM hygiene → liongard_metric over IAM users: console vs. programmatic access, MFA enabled, access-key age and last-used, attached policies. Angles to verify: `Users[?MfaEnabled==\`false\`]`, `Users[].AccessKeys[?Age > \`90\`]`. Root-account MFA status, if captured, leads the report regardless of the question.
   - Ownership test on every IAM user: keys belong to a person or a documented service. Cross-check unrecognized users against search_tickets and docs; an unclaimed IAM user with active keys is treated like an unexplained admin and escalates if recent.
   - S3 exposure → buckets flagged public or with permissive ACLs/policies. Public buckets are findings, not stats — flag with purpose known/unknown, route remediation to a technician.
   - Network exposure → security groups allowing 0.0.0.0/0 on management or database ports.
   - Census → EC2 (type, state), RDS, Lambda, load balancers. Stopped-but-provisioned and oversized resources are cost-review candidates — configuration signals only, never dollar figures.
   - Changes → liongard_detection / liongard_timeline for IAM changes, security-group modifications, new resources in an incident window.
   - Open-ended → liongard_query, verified with targeted metrics before anything actionable is reported.
3. Key age is risk, not proof of abuse — recommend rotation through the client's change process; never present rotation as done. Root-account and public-bucket findings lead the output whenever present.
4. Output: direct answer, evidence rows, dataprint as-of timestamp, inspected-scope statement. Plain-text ticket note on request. Degradation: no AWS inspector → ticket history and docs only, labeled "not instrumented"; meaningful AWS with no inspector is itself a recommendation.
```
