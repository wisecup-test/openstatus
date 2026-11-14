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
- **R-ENV-002** MANDATORY: Execute the lock-version grounding process (manifest discovery, build tool identification, exact resolved version check via lock/resolution artifact, official documentation lookup, and API existence confirmation) before writing code that uses a versioned library.
- **R-ENV-003** MANDATORY: Discover tool names, file names, commands, package managers, and version numbers from the project repository.

### Verify

```bash
# Discover and execute the project verification script from the repository manifest to validate environment configuration parsing.
# Discover and execute the static analysis and type checking script to verify environment schema type inference and variable references across modules.
```

**Accept when:**
- The verification test suite completes successfully without reporting environment schema parsing errors.
- The static type checker passes with zero diagnostics on exported environment configuration properties.

<enforcement>
Claude Code MUST NOT skip or defer verification. All application configuration derived from process environment variables must be validated using @t3-oss/env-core.
</enforcement>