---
version: 2
name: cross-cutting-payload-contracts-not-accept-unvalidated-31f5
description: "Adoption of zod for Payload Schema Validation: Payload Contracts Not Accept Unvalidated Arbitrary"
metadata:
  actual:
    adr_id: 31f55726-89bd-4243-ba7c-46acc2fb33b6
    status: accepted
    candidate_group_id: 4e2a1a98-2292-4ad7-96f3-b44c0c9979ca
    evidence:
      source: deep
  scope:
    root: packages/utils
    paths:
      - src/**
---

# Adoption of zod for Payload Schema Validation: Payload Contracts Not Accept Unvalidated Arbitrary

Scope: applies to `packages/utils/src/**`.

These rules are ALWAYS ACTIVE for shared utility modules, input schemas, and payload validation logic at service ingress and egress boundaries.

### Rules

- **R-ZOD-001** MUST_NOT: Payload contracts MUST_NOT accept unvalidated arbitrary object structures without explicit schema definition.

### Verify

```bash
# Discover and run the project test runner covering payload validation schemas
# Discover and execute the repository type checking script across shared contract packages
```

**Accept when:**
- All schema validation test cases execute and pass successfully without runtime parsing errors on valid payloads.
- Type checking verifies without errors across all modules importing the payload schemas.

<enforcement>
Claude Code MUST NOT skip or defer verification. Verification of schema test suites and static type checking is mandatory.
</enforcement>