---
version: 2
name: cross-cutting-shared-payload-definitions-across-servic-fc00
description: "Adoption of zod for Payload Schema Validation: Shared Payload Definitions Across Service Boundaries"
metadata:
  actual:
    adr_id: fc003192-23af-4f2d-907a-3694cdd73aa2
    status: accepted
    candidate_group_id: 4e2a1a98-2292-4ad7-96f3-b44c0c9979ca
    evidence:
      source: deep
  scope:
    root: packages/utils
    paths:
      - src/**
---

# Adoption of zod for Payload Schema Validation: Shared Payload Definitions Across Service Boundaries

Scope: applies to `packages/utils/src/**`.

These rules are ALWAYS ACTIVE for shared utility modules defining payload contracts, input schemas, and payload validation logic at service ingress and egress boundaries.

### Rules

- **R-ZOD-001** MUST: Shared payload definitions across service boundaries MUST use zod to define runtime validation schemas.

### Verify

```bash
# Discover the project test runner from repository configuration and execute the test suite covering payload validation schemas.
# Discover the repository type checking script and execute static analysis across all shared contract packages to ensure schema-derived types match expectations.
```

**Accept when:**
- All schema validation test cases execute and pass successfully without runtime parsing errors on valid payloads.
- Type checking verifies without errors across all modules importing the payload schemas.

<enforcement>
Claude Code MUST NOT skip or defer verification. Verification is mandatory.
</enforcement>