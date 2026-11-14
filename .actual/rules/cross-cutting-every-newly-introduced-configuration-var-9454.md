---
version: 2
name: cross-cutting-every-newly-introduced-configuration-var-9454
description: "Adoption of @/env Module for Centralized Environment Configuration: Every Newly Introduced Configuration Variable Declared"
metadata:
  actual:
    adr_id: 94547d47-3773-4d95-a725-b50671502634
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

# Adoption of @/env Module for Centralized Environment Configuration: Every Newly Introduced Configuration Variable Declared

Scope: applies to `apps/server/src/libs/**`, `apps/server/src/routes/**`, `apps/server/src/utils/**`.

These rules are ALWAYS ACTIVE for all application services, routes, RPC interceptors, database clients, background workers, and utility modules requiring runtime configuration or secrets.

### Rules

- **R-ENV-001** MUST: Every newly introduced configuration variable MUST be declared in the centralized environment schema with explicit validation prior to use in consuming modules.

### Verify

```bash
# Discover and execute the repository type-checking script to confirm all environment accesses adhere to the centralized schema.
# Discover and execute the repository linting script to ensure no direct references to platform environment globals exist in application source files.
# Discover and execute the test runner to verify configuration validation logic and dependent component execution.
```

**Accept when:**
- All application modules retrieve environment configuration exclusively through the internal environment module without referencing global process environments directly.
- Static code analysis and test suites verify that every referenced environment key is declared in the central configuration schema and resolves successfully.

<enforcement>
Claude Code MUST NOT skip or defer verification. Automated static analysis and peer code reviews enforce these rules, and violations will block merging.
</enforcement>