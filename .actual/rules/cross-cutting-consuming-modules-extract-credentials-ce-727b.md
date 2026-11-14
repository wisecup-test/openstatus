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
# Discover and execute repository type-checking, linting, and test scripts
typecheck_script=$(npm pkg get scripts.typecheck --json 2>/dev/null | tr -d '"')
[ -n "$typecheck_script" ] && npm run typecheck

lint_script=$(npm pkg get scripts.lint --json 2>/dev/null | tr -d '"')
[ -n "$lint_script" ] && npm run lint

test_script=$(npm pkg get scripts.test --json 2>/dev/null | tr -d '"')
[ -n "$test_script" ] && npm run test
```

**Accept when:**
- All application modules retrieve environment configuration exclusively through the internal environment module without referencing global process environments directly.
- Static code analysis and test suites verify that every referenced environment key is declared in the central configuration schema and resolves successfully.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>