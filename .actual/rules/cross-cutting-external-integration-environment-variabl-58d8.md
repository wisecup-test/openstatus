---
version: 2
name: cross-cutting-external-integration-environment-variabl-58d8
description: "Zod Schema Validation for Ingress Notification Data and Environment Configuration: External Integration Environment Variables Declared Verified"
metadata:
  actual:
    adr_id: 58d828dd-2efb-47d5-b36c-8c9ca1b846da
    status: accepted
    candidate_group_id: f65abc81-591a-4ca8-812c-a6af3fbfd0fc
    evidence:
      source: deep
  scope:
    root: packages/notifications/twillio-sms
    paths:
      - src/**
---

# Zod Schema Validation for Ingress Notification Data and Environment Configuration: External Integration Environment Variables Declared Verified

Scope: applies to `packages/notifications/twillio-sms/src/**`.

These rules are ALWAYS ACTIVE for all notification integration modules receiving serialized payload data and environment configuration modules initializing credentials and endpoints for external client integrations.

### Rules

- **R-ZOD-001** MUST: All external integration environment variables MUST be declared and verified using schema validation structures before consumption by external client adapters.
- **R-ZOD-002** MUST: Declare boundary schemas for both external configuration inputs and deserialized notification message objects.
- **R-ZOD-003** MUST: Handle schema parsing errors at ingress boundaries before constructing downstream request payloads.

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
Claude Code MUST NOT skip or defer verification. All boundary ingress payloads and environment configurations must be validated using schema parsing.
</enforcement>