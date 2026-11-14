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

These rules are ALWAYS ACTIVE for shared utility modules defining payload contracts and input schemas, and payload validation logic at service ingress and egress boundaries.

### Rules

- **R-ZOD-001** MUST: Shared payload definitions across service boundaries MUST use zod to define runtime validation schemas.
- **R-ZOD-002** MANDATORY: Prior to writing code that uses a versioned library, discover the dependency manifest, identify the build tool, inspect the repository lock or resolution artifact to determine the exact resolved version, look up official documentation for that exact version, confirm every API exists in that version, and re-run for version-sensitive behavior.
- **R-ZOD-003** MANDATORY: Compose complex payload schemas from shared atomic building blocks and common enumeration constants, and ensure optional configuration records utilize explicit key and value schema definitions to prevent untyped mapping.

### Verify

```bash
# Discover and execute the project test runner covering payload validation schemas
# Discover and execute the repository type checking script across shared contract packages
```

**Accept when:**
- All schema validation test cases execute and pass successfully without runtime parsing errors on valid payloads.
- Type checking verifies without errors across all modules importing the payload schemas.

<enforcement>
Claude Code MUST NOT skip or defer verification. Violation handling: Pull requests introducing unvalidated payload contracts or bypassing schema validation must be rejected.
</enforcement>