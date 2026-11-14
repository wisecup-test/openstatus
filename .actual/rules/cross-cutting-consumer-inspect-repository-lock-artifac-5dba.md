---
version: 2
name: cross-cutting-consumer-inspect-repository-lock-artifac-5dba
description: "@t3-oss/env-core Environment Variable Schema Validation: Consumer Inspect Repository Lock Artifact Determine"
metadata:
  actual:
    adr_id: 5dba7d87-9249-4be4-b072-bcb9617e6526
    status: accepted
    candidate_group_id: 3d0a80d9-ab93-46e2-9085-ac3694326c96
    evidence:
      source: deep
  scope:
    root: packages/analytics
---

# @t3-oss/env-core Environment Variable Schema Validation: Consumer Inspect Repository Lock Artifact Determine

Scope: applies to `packages/analytics/**`.

These rules are ALWAYS ACTIVE for modules and packages that define, export, or consume runtime environment configurations, analytics tokens, or service credentials.

### Rules

- **R-T3E-001** MUST: The consumer MUST inspect the repository lock artifact to determine the exact resolved version of @t3-oss/env-core and verify API compatibility before implementing or modifying environment schemas.
- **R-T3E-002** MUST: Construct environment configuration modules by importing the schema generator from @t3-oss/env-core and declaring type validators using zod for each expected variable.
- **R-T3E-003** MUST: Export the validated configuration object as the single point of entry for consuming modules rather than exposing direct references to runtime environment properties.

### Verify

```bash
# Discover and execute the project test runner to verify environment schema parsing
# Discover and run the project type checking script to ensure exported environment configurations satisfy static type signatures
# Discover and execute the project linting and boundary verification scripts to confirm no direct accesses to unvalidated runtime environment variables remain
```

**Accept when:**
- Environment variable validation successfully initializes and parses required configuration entries without throwing runtime schema errors.
- All modules requiring runtime configuration import validated objects rather than accessing raw environment properties directly.
- Type checking and automated test suites pass without validation or typing diagnostics.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>