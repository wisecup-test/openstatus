---
version: 2
name: cross-cutting-application-components-not-read-global-5ecf
description: "Adoption of @/env Module for Centralized Environment Configuration: Application Components Not Read Global Platform"
metadata:
  actual:
    adr_id: 5ecfeed2-b29e-475f-8111-198f3dc6358d
    status: accepted
    candidate_group_id: 769d826a-736d-4c06-974b-1987d15d8aa7
    evidence:
      source: deep
  scope:
    root: apps/server
    paths:
      - src/libs/**
      - src/routes/**
      - src/utils/**
---

# Adoption of @/env Module for Centralized Environment Configuration: Application Components Not Read Global Platform

Scope: applies to `apps/server/src/libs/**`, `apps/server/src/routes/**`, `apps/server/src/utils/**`.

These rules are ALWAYS ACTIVE for all application components, server routes, RPC interceptors, client initializers, and utility modules requiring runtime configuration or secrets.

### Rules

- **R-ENV-001** MUST_NOT: Application components MUST NOT read from global platform environment variables directly outside of the designated environment definition module.

### Verify

```bash
# Discover and execute repository type-checking script
# Discover and execute repository linting script
# Discover and execute the test runner
```

**Accept when:**
- All application modules retrieve environment configuration exclusively through the internal environment module without referencing global process environments directly.
- Static code analysis and test suites verify that every referenced environment key is declared in the central configuration schema and resolves successfully.

<enforcement>
Claude Code MUST NOT skip or defer verification. Automated static analysis and code review check that direct references to platform environment globals outside the designated configuration module are prohibited.
</enforcement>