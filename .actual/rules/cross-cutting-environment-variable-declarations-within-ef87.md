---
version: 2
name: cross-cutting-environment-variable-declarations-within-ef87
description: "@t3-oss/env-core for Environment Variable Validation: Environment Variable Declarations Within Oss Env"
metadata:
  actual:
    adr_id: ef87c963-2678-468c-970c-ea6356a5e3b6
    status: accepted
    candidate_group_id: 31da84ba-1169-423b-8506-9a385a0a9140
    evidence:
      source: deep
  scope:
    root: packages/notifications/pagerduty
    paths:
      - src/**
---

# @t3-oss/env-core for Environment Variable Validation: Environment Variable Declarations Within Oss Env

Scope: applies to `packages/notifications/pagerduty/src/**`.

These rules are ALWAYS ACTIVE for modules defining runtime configuration and external service integration credentials.

### Rules

- **R-ENV-001** MUST: Environment variable declarations within @t3-oss/env-core MUST define explicit zod schemas for all expected runtime variables.

### Verify

```bash
# Discover and execute the project test runner across all packages containing environment configuration modules
# Discover and run the project type-checking and linting suites to ensure all environment variable usages reference the validated configuration object
```

**Accept when:**
- All environment configuration test suites pass without validation or runtime errors.
- Static type checking passes with zero type errors related to environment variable references.

<enforcement>
Claude Code MUST NOT skip or defer verification. All pull requests containing direct unvalidated environment variable access or failing schema validation tests are blocked from merging.
</enforcement>