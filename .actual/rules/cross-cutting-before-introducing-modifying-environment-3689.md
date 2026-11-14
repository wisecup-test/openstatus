---
version: 2
name: cross-cutting-before-introducing-modifying-environment-3689
description: "@t3-oss/env-core for Environment Variable Validation: Before Introducing Modifying Environment Schemas Oss"
metadata:
  actual:
    adr_id: 36891357-7ec0-4783-b4f1-8afa4781361a
    status: accepted
    candidate_group_id: 31da84ba-1169-423b-8506-9a385a0a9140
    evidence:
      source: deep
  scope:
    root: packages/notifications/pagerduty
    paths:
      - src/**
---

# @t3-oss/env-core for Environment Variable Validation: Before Introducing Modifying Environment Schemas Oss

Scope: applies to `packages/notifications/pagerduty/src/**`.

These rules are ALWAYS ACTIVE for modules defining runtime configuration and external service integration credentials.

### Rules

- **R-ENV-001** MUST: Before introducing or modifying environment schemas with @t3-oss/env-core, developers MUST inspect the repository lock artifact to determine the exact resolved dependency version and confirm that all utilized APIs exist within official documentation for that version.

### Verify

```bash
# Discover and execute the project test runner across all packages containing environment configuration modules
# Discover and run the project type-checking and linting suites to ensure all environment variable usages reference the validated configuration object
```

**Accept when:**
- All environment configuration test suites pass without validation or runtime errors.
- Static type checking passes with zero type errors related to environment variable references.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>