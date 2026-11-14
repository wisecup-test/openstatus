---
version: 2
name: cross-cutting-downstream-payload-data-types-inferred-5342
description: "Zod Schema Validation for Ingress Notification Data and Environment Configuration: Downstream Payload Data Types Inferred Directly"
metadata:
  actual:
    adr_id: 53424857-f306-4b77-9dd0-0fb04cec73f7
    status: accepted
    candidate_group_id: f65abc81-591a-4ca8-812c-a6af3fbfd0fc
    evidence:
      source: deep
  scope:
    root: packages/notifications/twillio-sms
    paths:
      - src/**
---

# Zod Schema Validation for Ingress Notification Data and Environment Configuration: Downstream Payload Data Types Inferred Directly

Scope: applies to `packages/notifications/twillio-sms/src/**`.

These rules are ALWAYS ACTIVE for all notification integration modules receiving serialized payload data from external or cross-service boundaries, and all environment configuration modules initializing credentials and endpoints for external client integrations.

### Rules

- **R-ZOD-001** SHOULD: Downstream payload data types SHOULD be inferred directly from the corresponding validation schemas rather than manually declared.
- **R-ZOD-002** MANDATORY: Declare boundary schemas for both external configuration inputs and deserialized notification message objects.
- **R-ZOD-003** MANDATORY: Handle schema parsing errors at ingress boundaries before constructing downstream request payloads.

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
Claude Code MUST NOT skip or defer verification. Continuous integration static analysis checks and peer code reviews enforce schema validation on all boundary data ingress points.
</enforcement>