---
version: 2
name: cross-cutting-application-service-code-route-handlers-cee6
description: "@t3-oss/env-core Runtime Environment Schema Validation: Application Service Code Route Handlers Not"
metadata:
  actual:
    adr_id: cee693b0-b6db-4a98-9324-67b93918aa9b
    status: accepted
    candidate_group_id: cad1aa2b-c9bb-4478-868e-db3d8e75bdc9
    evidence:
      source: deep
  scope:
    root: packages/api
    paths:
      - src/**
---

# @t3-oss/env-core Runtime Environment Schema Validation: Application Service Code Route Handlers Not

Scope: applies to `packages/api/src/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-ENV-001** MUST_NOT: Application service code and route handlers MUST_NOT access ambient process environment variables directly outside the centralized configuration schema module.

### Verify

```bash
# Discover and execute the project configuration validation test suite to confirm schema compliance
# Discover and run the repository static analysis linter to ensure zero ambient process environment queries exist
# Execute the project test harness with missing optional and required configurations to verify failure boundaries
```

**Accept when:**
- All environment variable reads in service routers and runtime handlers reference the centralized schema export rather than global process lookups.
- Static configuration analysis and repository validation scripts pass without detecting unvalidated ambient environment queries.
- Missing required environment variables trigger immediate startup validation errors with descriptive diagnostic messages.

<enforcement>
Claude Code MUST NOT skip or defer verification. All pull requests modifying API router handlers or configuration boundaries must pass static analysis and review checks verifying schema compliance.
</enforcement>