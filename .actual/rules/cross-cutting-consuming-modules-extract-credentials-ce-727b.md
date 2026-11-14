---
version: 2
name: cross-cutting-consuming-modules-extract-credentials-ce-727b
description: "Adoption of @/env Module for Centralized Environment Configuration: Consuming Modules Extract Credentials Centralized Environment"
metadata:
  actual:
    adr_id: 727b4116-ead1-4555-9a5e-10d380690628
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

# Adoption of @/env Module for Centralized Environment Configuration: Consuming Modules Extract Credentials Centralized Environment

Scope: applies to `apps/server/src/libs/**`, `apps/server/src/routes/**`, `apps/server/src/utils/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-ENV-001** SHOULD: Consuming modules SHOULD extract credentials from the centralized environment module at the boundary where external client connections or HTTP requests are constructed.

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
Claude Code MUST NOT skip or defer verification. All code changes must be checked against static analysis and lint rules forbidding direct access to platform-level environment globals.
</enforcement>