---
version: 2
name: cross-cutting-application-components-not-access-raw-cb44
description: "@t3-oss/env-core for Environment Variable Validation: Application Components Not Access Raw Process"
metadata:
  actual:
    adr_id: cb44f313-6caa-4963-ac38-c638bc0a2b37
    status: accepted
    candidate_group_id: 31da84ba-1169-423b-8506-9a385a0a9140
    evidence:
      source: deep
  scope:
    root: packages/notifications/pagerduty
    paths:
      - src/**
---

# @t3-oss/env-core for Environment Variable Validation: Application Components Not Access Raw Process

Scope: applies to `packages/notifications/pagerduty/src/**`.

These rules are ALWAYS ACTIVE for modules defining runtime configuration and external service integration credentials.

### Rules

- **R-ENV-001** MUST_NOT: Application components MUST_NOT access raw process environment variables directly when an @t3-oss/env-core validated schema is established.

### Verify

```bash
# Discover and execute the project test runner across packages containing environment configuration modules
# Discover and run the project type-checking and linting suites
```

**Accept when:**
- All environment configuration test suites pass without validation or runtime errors.
- Static type checking passes with zero type errors related to environment variable references.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>