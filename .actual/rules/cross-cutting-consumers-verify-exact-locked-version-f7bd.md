---
version: 2
name: cross-cutting-consumers-verify-exact-locked-version-f7bd
description: "@t3-oss/env-core Secret and Environment Variable Validation: Consumers Verify Exact Locked Version Environment"
metadata:
  actual:
    adr_id: f7bda1cb-19d2-4308-9718-9e8f0bdfd5fd
    status: accepted
    candidate_group_id: a46b086e-afb5-4b0c-9937-cd3c83432804
    evidence:
      source: deep
  scope:
    root: packages/api
    paths:
      - src/**
---

# @t3-oss/env-core Secret and Environment Variable Validation: Consumers Verify Exact Locked Version Environment

Scope: applies to `packages/api/src/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-ENV-001** MUST: Consumers MUST verify the exact locked version of the environment validation library in the project lock artifact prior to implementing or extending schema definitions.

### Verify

```bash
# Discover the repository verification script from the project manifest and execute the test suite to validate schema parsing
# Execute the repository static analysis and linting script to verify absence of direct runtime environment reads outside the environment configuration module
```

**Accept when:**
- All server-side secrets and environment configuration variables are declared and validated through the centralized schema module
- Direct untyped access to runtime environment variables across application routers and service handlers is eliminated

<enforcement>
Claude Code MUST NOT skip or defer verification. All server secrets and environment variables must be validated through the centralized schema module and verified by static analysis and tests.
</enforcement>