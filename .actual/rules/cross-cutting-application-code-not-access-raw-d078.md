---
version: 2
name: cross-cutting-application-code-not-access-raw-d078
description: "@t3-oss/env-core Environment Variable Schema Validation: Application Code Not Access Raw Runtime"
metadata:
  actual:
    adr_id: d078d754-13cd-44a0-a0f6-aec10344973e
    status: accepted
    candidate_group_id: 3d0a80d9-ab93-46e2-9085-ac3694326c96
    evidence:
      source: deep
  scope:
    root: packages/analytics
---

# @t3-oss/env-core Environment Variable Schema Validation: Application Code Not Access Raw Runtime

Scope: applies to `packages/analytics/**`.

These rules are ALWAYS ACTIVE for modules and packages that define, export, or consume runtime environment configurations, analytics tokens, or service credentials.

### Rules

- **R-ENV-001** MUST_NOT: Application code MUST_NOT access raw runtime environment variables directly when a validated schema definition exists.

### Verify

```bash
# Discover the project test script from the dependency manifest and execute the test runner to verify environment schema parsing.
# Discover and run the project type checking script to ensure exported environment configurations satisfy static type signatures.
# Discover and execute the project linting and boundary verification scripts to confirm no direct accesses to unvalidated runtime environment variables remain.
```

**Accept when:**
- Environment variable validation successfully initializes and parses required configuration entries without throwing runtime schema errors.
- All modules requiring runtime configuration import validated objects rather than accessing raw environment properties directly.
- Type checking and automated test suites pass without validation or typing diagnostics.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>