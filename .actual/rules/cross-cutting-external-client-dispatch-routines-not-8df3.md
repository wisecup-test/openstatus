---
version: 2
name: cross-cutting-external-client-dispatch-routines-not-8df3
description: "Zod Schema Validation for Ingress Notification Data and Environment Configuration: External Client Dispatch Routines Not Consume"
metadata:
  actual:
    adr_id: 8df37a34-d910-400b-b458-25fc254bbf84
    status: accepted
    candidate_group_id: f65abc81-591a-4ca8-812c-a6af3fbfd0fc
    evidence:
      source: deep
  scope:
    root: packages/notifications/twillio-sms
    paths:
      - src/**
---

# Zod Schema Validation for Ingress Notification Data and Environment Configuration: External Client Dispatch Routines Not Consume

Scope: applies to `packages/notifications/twillio-sms/src/**`.

These rules are ALWAYS ACTIVE for all notification integration modules receiving serialized payload data and environment configuration modules initializing credentials.

### Rules

- **R-ZOD-001** MUST_NOT: External client dispatch routines MUST NOT consume raw deserialized data without passing through schema validation.

### Verify

```bash
# Discover and run the project type checking script to verify schema type inference across boundary consumers.
# Discover and execute the project unit and integration test suite to validate boundary schema parsing and rejection behavior.
# Discover and run the project linting and static analysis tool to detect unvalidated ingress payload parsing.
```

**Accept when:**
- All boundary ingress payloads and environment configurations pass schema parsing before execution proceeds.
- Static type checking verifies that downstream integration calls consume validated schema output types.
- Automated verification suites confirm parsing failures halt execution with boundary errors prior to external dispatch.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>