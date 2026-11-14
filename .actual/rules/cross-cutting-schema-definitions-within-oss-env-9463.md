---
version: 2
name: cross-cutting-schema-definitions-within-oss-env-9463
description: "Adoption of @t3-oss/env-core for Environment Variable Validation: Schema Definitions Within Oss Env Core"
metadata:
  actual:
    adr_id: 94637c00-3376-49fe-8627-4e0162d82edc
    status: accepted
    candidate_group_id: cadf5e56-4866-48a6-ab51-36a4a0721d5d
    evidence:
      source: deep
  scope:
    root: apps/screenshot-service
    paths:
      - src/**
---

# Adoption of @t3-oss/env-core for Environment Variable Validation: Schema Definitions Within Oss Env Core

Scope: applies to `apps/screenshot-service/src/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-ENV-001** MUST: Schema definitions within @t3-oss/env-core MUST specify explicit type validation primitives for every declared variable to ensure strict runtime shape conformance.

### Verify

```bash
# Discover the project verification script from the repository manifest and execute the test runner to validate environment configuration parsing.
# Discover the static analysis and type checking script to verify environment schema type inference and variable references across modules.
```

**Accept when:**
- The verification test suite completes successfully without reporting environment schema parsing errors.
- The static type checker passes with zero diagnostics on exported environment configuration properties.

<enforcement>
Claude Code MUST NOT skip or defer verification. Automated build pipelines, static analysis, and peer code reviews enforce compliance.
</enforcement>