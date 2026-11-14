---
version: 2
name: cross-cutting-consumers-discover-repository-dependency-5d8e
description: "Adoption of zod for Payload Schema Validation: Consumers Discover Repository Dependency Manifest Lock"
metadata:
  actual:
    adr_id: 5d8e1d79-ea41-4d4b-a675-fb3dccc153bf
    status: accepted
    candidate_group_id: 4e2a1a98-2292-4ad7-96f3-b44c0c9979ca
    evidence:
      source: deep
  scope:
    root: packages/utils
    paths:
      - src/**
---

# Adoption of zod for Payload Schema Validation: Consumers Discover Repository Dependency Manifest Lock

Scope: applies to `packages/utils/src/**`.

These rules are ALWAYS ACTIVE for shared utility modules defining payload contracts, input schemas, and payload validation logic at service ingress and egress boundaries.

### Rules

- **R-ZOD-001** MUST: Consumers MUST discover the repository dependency manifest and lock artifact to resolve the exact locked version of zod before implementing schema definitions.

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