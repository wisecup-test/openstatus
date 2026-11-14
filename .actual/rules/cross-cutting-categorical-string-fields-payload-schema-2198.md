---
version: 2
name: cross-cutting-categorical-string-fields-payload-schema-2198
description: "Adoption of zod for Payload Schema Validation: Categorical String Fields Payload Schemas Constrained"
metadata:
  actual:
    adr_id: 21981417-6cc7-4eb8-9e3b-cf930bfff2b9
    status: accepted
    candidate_group_id: 4e2a1a98-2292-4ad7-96f3-b44c0c9979ca
    evidence:
      source: deep
  scope:
    root: packages/utils
    paths:
      - src/**
---

# Adoption of zod for Payload Schema Validation: Categorical String Fields Payload Schemas Constrained

Scope: applies to `packages/utils/src/**`.

These rules are ALWAYS ACTIVE for all shared utility modules defining payload contracts and input schemas, and payload validation logic at service ingress and egress boundaries.

### Rules

- **R-ZOD-001** MUST: Categorical string fields in payload schemas MUST be constrained using z.enum with centralized enumeration definitions.

### Verify

```bash
# Discover and execute project test runner for payload validation schemas
# Discover and execute static analysis across shared contract packages
```

**Accept when:**
- All schema validation test cases execute and pass successfully without runtime parsing errors on valid payloads.
- Type checking verifies without errors across all modules importing the payload schemas.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>