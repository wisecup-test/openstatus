---
version: 2
name: cross-cutting-development-only-debug-utilities-use-4de0
description: "console.error Logging: Development Only Debug Utilities Use Console"
metadata:
  actual:
    adr_id: 4de03ab1-9ddc-4931-b3f4-47eda7292545
    status: accepted
    candidate_group_id: 67cb9726-3590-4f64-8f42-95e6f1534ece
    evidence:
      source: deep
  scope:
    root: packages/assertions
    paths:
      - src/**
---

# console.error Logging: Development Only Debug Utilities Use Console

Scope: applies to `packages/assertions/src/**`.

These rules are ALWAYS ACTIVE for runtime payload parsing and error handling in assertion modules and diagnostic output generated during validation failure paths.

### Rules

- **R-46-001** MAY: Development-only debug utilities MAY use console.error during local debugging sessions when gated behind development environment conditional flags.

### Verify

```bash
# Discover and execute project linting/static analysis scripts to verify no unauthorized console invocations exist
# Discover and run the project test suite using build orchestration scripts to verify assertion parsing error paths
```

**Accept when:**
- The static analysis verification script executes without reporting unauthorized console.error usage in production modules.
- All unit and integration tests for payload parsing error handling pass successfully.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>