---
version: 2
name: cross-cutting-application-components-requiring-runtime-4ea7
description: "Adoption of @/env Module for Centralized Environment Configuration: Application Components Requiring Runtime Configuration External"
metadata:
  actual:
    adr_id: 4ea79ed1-246c-4941-977c-9f66a3b45399
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

# Adoption of @/env Module for Centralized Environment Configuration: Application Components Requiring Runtime Configuration External

Scope: applies to `apps/server/src/libs/**`, `apps/server/src/routes/**`, `apps/server/src/utils/**`.

These rules are ALWAYS ACTIVE for all application services, routes, RPC interceptors, client initializers, and utility modules requiring runtime configuration or external service credentials.

### Rules

- **R-ENV-001** MUST: Application components requiring runtime configuration or external service credentials MUST access them exclusively through the @/env internal module.

### Verify

```bash
# Discover and execute the repository type-checking script
# Discover and execute the repository linting script
# Discover and execute the test runner
```

**Accept when:**
- All application modules retrieve environment configuration exclusively through the internal environment module without referencing global process environments directly.
- Static code analysis and test suites verify that every referenced environment key is declared in the central configuration schema and resolves successfully.

<enforcement>
Claude Code MUST NOT skip or defer verification. All pull requests and code modifications must adhere to centralized environment configuration rules.
</enforcement>