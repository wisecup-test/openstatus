---
version: 2
name: cross-cutting-before-adopting-external-logging-librari-920f
description: "console.error Logging: Before Adopting External Logging Libraries Replace"
metadata:
  actual:
    adr_id: 920fbe68-0f22-4b7d-9db5-b5278fe118f4
    status: accepted
    candidate_group_id: 67cb9726-3590-4f64-8f42-95e6f1534ece
    evidence:
      source: deep
  scope:
    root: packages/assertions
    paths:
      - src/**
---

# console.error Logging: Before Adopting External Logging Libraries Replace

Scope: applies to `packages/assertions/src/**`.

These rules are ALWAYS ACTIVE for runtime payload parsing and error handling in assertion modules, and diagnostic output generated during validation failure paths.

### Rules

- **R-LOG-001** MUST: Before adopting external logging libraries to replace runtime console calls, consumers MUST inspect the repository lock artifact to verify the exact resolved version against authorized dependency records.

### Verify

```bash
# Discover the project linting and static analysis scripts from the dependency manifest and execute them
# Example discovery & execution pattern:
# npm run lint || yarn lint || pnpm lint

# Discover and run the project test suite using project build orchestration scripts
# npm test || yarn test || pnpm test
```

**Accept when:**
- The static analysis verification script executes without reporting unauthorized console.error usage in production modules.
- All unit and integration tests for payload parsing error handling pass successfully.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>