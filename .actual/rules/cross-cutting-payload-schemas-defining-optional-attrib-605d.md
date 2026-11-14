---
version: 2
name: cross-cutting-payload-schemas-defining-optional-attrib-605d
description: "Adoption of zod for Payload Schema Validation: Payload Schemas Defining Optional Attributes Fallback"
metadata:
  actual:
    adr_id: 605d1985-788c-4bfb-888f-d6cef6e29e4e
    status: accepted
    candidate_group_id: 4e2a1a98-2292-4ad7-96f3-b44c0c9979ca
    evidence:
      source: deep
  scope:
    root: packages/utils
    paths:
      - src/**
---

# Adoption of zod for Payload Schema Validation: Payload Schemas Defining Optional Attributes Fallback

Scope: applies to `packages/utils/src/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-ZOD-001** MUST: Payload schemas defining optional attributes with fallback values MUST use the prefault modifier to guarantee deterministic defaults upon parsing.

### Verify

```bash
# Discover and run the test suite covering payload validation schemas
# Discover and run static analysis across all shared contract packages to ensure schema-derived types match expectations
```

**Accept when:**
- All schema validation test cases execute and pass successfully without runtime parsing errors on valid payloads.
- Type checking verifies without errors across all modules importing the payload schemas.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>