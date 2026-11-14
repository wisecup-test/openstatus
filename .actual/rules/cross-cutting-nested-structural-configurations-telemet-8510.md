---
version: 2
name: cross-cutting-nested-structural-configurations-telemet-8510
description: "Adoption of zod for Payload Schema Validation: Nested Structural Configurations Telemetry Headers Payloads"
metadata:
  actual:
    adr_id: 85106a82-30e3-41db-a484-9fdf0f21953a
    status: accepted
    candidate_group_id: 4e2a1a98-2292-4ad7-96f3-b44c0c9979ca
    evidence:
      source: deep
  scope:
    root: packages/utils
    paths:
      - src/**
---

# Adoption of zod for Payload Schema Validation: Nested Structural Configurations Telemetry Headers Payloads

Scope: applies to `packages/utils/src/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-ZOD-001** SHOULD: Nested structural configurations and telemetry headers in payloads SHOULD be modeled using z.object and z.record to maintain explicit key-value constraints.

### Verify

```bash
# Discover and execute the project test runner covering payload validation schemas
# Discover and execute repository type checking across shared contract packages
```

**Accept when:**
- All schema validation test cases execute and pass successfully without runtime parsing errors on valid payloads.
- Type checking verifies without errors across all modules importing the payload schemas.

<enforcement>
Claude Code MUST NOT skip or defer verification. All pull requests introducing unvalidated payload contracts or bypassing schema validation must be rejected.
</enforcement>