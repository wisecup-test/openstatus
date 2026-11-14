---
version: 2
name: cross-cutting-when-modifying-implementing-dependencies-818c
description: "Adoption of @/env Module for Centralized Environment Configuration: When Modifying Implementing Dependencies That Support"
metadata:
  actual:
    adr_id: 818c2308-8ea2-4882-a245-7bee8b3db205
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

# Adoption of @/env Module for Centralized Environment Configuration: When Modifying Implementing Dependencies That Support

Scope: applies to `apps/server/src/libs/**`, `apps/server/src/routes/**`, `apps/server/src/utils/**`.

These rules are ALWAYS ACTIVE for all application services, routes, RPC interceptors, client initializers, database clients, background workers, and utility modules requiring runtime configuration or secrets.

### Rules

- **R-ENV-001** MUST: When modifying or implementing dependencies that support the environment validation schema, the consumer MUST inspect the repository lock or resolution artifact to determine the exact locked version before implementation.

### Verify

```bash
# Discover and execute repository type-checking, linting, and test scripts
# Example discovery workflow:
if [ -f "package.json" ]; then
  npm run typecheck || npx tsc --noEmit
  npm run lint
  npm test
fi
```

**Accept when:**
- All application modules retrieve environment configuration exclusively through the internal environment module without referencing global process environments directly.
- Static code analysis and test suites verify that every referenced environment key is declared in the central configuration schema and resolves successfully.

<enforcement>
Claude Code MUST NOT skip or defer verification. Automated static analysis checks and peer code reviews verify that all configuration values are defined within the centralized environment module schema and platform globals are not accessed directly.
</enforcement>