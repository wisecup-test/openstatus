---
version: 2
name: cross-cutting-consuming-package-modules-not-bypass-9b50
description: "Environment Variable Schema Validation with @t3-oss/env-core: Consuming Package Modules Not Bypass Validated"
metadata:
  actual:
    adr_id: 9b50ad99-7efc-428d-a97c-997342301738
    status: accepted
    candidate_group_id: 2c36a9ad-0737-47e4-90e0-574d2377a7a5
    evidence:
      source: deep
  scope:
    root: packages/db
---

# Environment Variable Schema Validation with @t3-oss/env-core: Consuming Package Modules Not Bypass Validated

Scope: applies to `packages/db/**`.

These rules are ALWAYS ACTIVE for configuration management and environment variable access within backend and database workspace packages, as well as initialization of runtime credentials, database connection parameters, and operational environment flags.

### Rules

- **R-ENV-001** MUST_NOT: Consuming package modules MUST NOT bypass the validated environment schema to read raw process environment variables directly.

### Verify

```bash
# Discover and run the project's static analysis and type verification suite
# to confirm that all references to environment variables utilize the validated configuration object.

# Discover and run the repository test suite to verify that invalid or missing
# environment configurations trigger module-load validation errors as expected.
```

**Accept when:**
- Static verification confirms zero unvalidated references to raw process environment variables within the package boundary.
- Module initialization successfully parses valid environment variables and throws descriptive validation errors when required variables are absent or invalid.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>