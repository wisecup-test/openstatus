---
version: 2
name: cross-cutting-application-configuration-derived-proces-c037
description: "Adoption of @t3-oss/env-core for Environment Variable Validation: Application Configuration Derived Process Environment Variables"
metadata:
  actual:
    adr_id: c03707d8-eb57-4807-a828-7f50e56c4de6
    status: accepted
    candidate_group_id: cadf5e56-4866-48a6-ab51-36a4a0721d5d
    evidence:
      source: deep
  scope:
    root: apps/screenshot-service
    paths:
      - src/**
---

# Adoption of @t3-oss/env-core for Environment Variable Validation: Application Configuration Derived Process Environment Variables

Scope: applies to `apps/screenshot-service/src/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-ENV-001** MUST: All application configuration derived from process environment variables MUST be declared and validated using @t3-oss/env-core schemas prior to consumption.

### Verify

```bash
# Discover and execute project verification and test scripts from the repository manifest
# Example discovery workflow:
# 1. Inspect package.json for test/typecheck scripts
# 2. Run the package manager test runner and type checker
npm test
npm run typecheck
```

**Accept when:**
- The verification test suite completes successfully without reporting environment schema parsing errors.
- The static type checker passes with zero diagnostics on exported environment configuration properties.

<enforcement>
Claude Code MUST NOT skip or defer verification. Continuous integration test suites and automated static type checking pipelines verify module imports and configuration property access.
</enforcement>