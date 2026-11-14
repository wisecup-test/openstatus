---
version: 2
name: cross-cutting-parsing-failures-within-assertion-routin-a29e
description: "console.error Logging: Parsing Failures Within Assertion Routines Propagate"
metadata:
  actual:
    adr_id: a29e570a-186c-494c-a0bb-35dabc9d1179
    status: accepted
    candidate_group_id: 67cb9726-3590-4f64-8f42-95e6f1534ece
    evidence:
      source: deep
  scope:
    root: packages/assertions
    paths:
      - src/**
---

# console.error Logging: Parsing Failures Within Assertion Routines Propagate

Scope: applies to `packages/assertions/src/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-46-001** SHOULD: Parsing failures within assertion routines SHOULD propagate structured diagnostic exceptions to callers instead of reporting errors solely through standard output streams.

### Verify

```bash
# Discover and execute project linting / static analysis scripts to verify no unauthorized console invocations exist
# Discover and run the project test suite using build orchestration scripts
```

**Accept when:**
- The static analysis verification script executes without reporting unauthorized console.error usage in production modules.
- All unit and integration tests for payload parsing error handling pass successfully.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>