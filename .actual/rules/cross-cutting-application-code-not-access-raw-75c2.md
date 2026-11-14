---
version: 2
name: cross-cutting-application-code-not-access-raw-75c2
description: "Adoption of @t3-oss/env-core for Environment Variable Validation: Application Code Not Access Raw Process"
metadata:
  actual:
    adr_id: 75c2d55e-cecc-4496-b427-26f7a26aecf5
    status: accepted
    candidate_group_id: cadf5e56-4866-48a6-ab51-36a4a0721d5d
    evidence:
      source: deep
  scope:
    root: apps/screenshot-service
    paths:
      - src/**
---

# Adoption of @t3-oss/env-core for Environment Variable Validation: Application Code Not Access Raw Process

Scope: applies to `apps/screenshot-service/src/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-ENV-001** MUST_NOT: Application code MUST_NOT access raw process environment variables directly outside the centralized @t3-oss/env-core schema definition module.

### Verify

```bash
# Discover the project verification script from the repository manifest and execute the test runner to validate environment configuration parsing.
# Discover the static analysis and type checking script to verify environment schema type inference and variable references across modules.
```

**Accept when:**
- The verification test suite completes successfully without reporting environment schema parsing errors.
- The static type checker passes with zero diagnostics on exported environment configuration properties.

<enforcement>
Claude Code MUST NOT skip or defer verification. Automated static type checking, linting, and continuous integration test suites enforce these rules.
</enforcement>