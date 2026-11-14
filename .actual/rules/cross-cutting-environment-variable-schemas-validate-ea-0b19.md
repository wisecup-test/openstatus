---
version: 2
name: cross-cutting-environment-variable-schemas-validate-ea-0b19
description: "Environment Variable Schema Validation with @t3-oss/env-core: Environment Variable Schemas Validate Each Configuration"
metadata:
  actual:
    adr_id: 0b19665b-e03d-40db-ba6b-81a2840c2896
    status: accepted
    candidate_group_id: 2c36a9ad-0737-47e4-90e0-574d2377a7a5
    evidence:
      source: deep
  scope:
    root: packages/db
---

# Environment Variable Schema Validation with @t3-oss/env-core: Environment Variable Schemas Validate Each Configuration

Scope: applies to `packages/db/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-ENV-001** MUST: Environment variable schemas MUST validate each configuration source using zod type constraints prior to exposing configuration values to consuming modules.

### Verify

```bash
# Discover and run the project's static analysis and type verification suite to confirm that all references to environment variables utilize the validated configuration object.
# Discover and run the repository test suite to verify that invalid or missing environment configurations trigger module-load validation errors as expected.
```

**Accept when:**
- Static verification confirms zero unvalidated references to raw process environment variables within the package boundary.
- Module initialization successfully parses valid environment variables and throws descriptive validation errors when required variables are absent or invalid.

<enforcement>
Claude Code MUST NOT skip or defer verification. All pull requests introducing direct process environment access or unvalidated configuration reads are blocked until reconciled with the environment schema.
</enforcement>