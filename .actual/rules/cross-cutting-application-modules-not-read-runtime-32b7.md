---
version: 2
name: cross-cutting-application-modules-not-read-runtime-32b7
description: "@t3-oss/env-core Secret and Environment Variable Validation: Application Modules Not Read Runtime Environment"
metadata:
  actual:
    adr_id: 32b749db-ad2c-4ad4-9ae3-37783e2065a7
    status: accepted
    candidate_group_id: a46b086e-afb5-4b0c-9937-cd3c83432804
    evidence:
      source: deep
  scope:
    root: packages/api
    paths:
      - src/**
---

# @t3-oss/env-core Secret and Environment Variable Validation: Application Modules Not Read Runtime Environment

Scope: applies to `packages/api/src/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-ENV-001** MUST_NOT: Application modules MUST NOT read runtime environment variables directly outside the centralized environment configuration module.

### Verify

```bash
# Discover the repository verification script from the project manifest and execute the test suite to validate schema parsing
# Execute the repository static analysis and linting script to verify absence of direct runtime environment reads outside the environment configuration module
```

**Accept when:**
- All server-side secrets and environment configuration variables are declared and validated through the centralized schema module
- Direct untyped access to runtime environment variables across application routers and service handlers is eliminated

<enforcement>
Claude Code MUST NOT skip or defer verification. Verification by static analysis, lint checks scanning for raw environment variable reads, and automated test suite execution is mandatory.
</enforcement>