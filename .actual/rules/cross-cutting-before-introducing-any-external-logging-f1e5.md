---
version: 2
name: cross-cutting-before-introducing-any-external-logging-f1e5
description: "Console API for Diagnostic and Operational Logging: Before Introducing Any External Logging Dependency"
metadata:
  actual:
    adr_id: f1e59200-d985-4ddf-9669-24448ed027a6
    status: accepted
    candidate_group_id: b26fa19d-bfc7-4c59-b7c9-13cbbd8e23ce
    evidence:
      source: deep
  scope:
    root: packages/ui
    paths:
      - scripts/**
      - src/hooks/**
---

# Console API for Diagnostic and Operational Logging: Before Introducing Any External Logging Dependency

Scope: applies to `packages/ui/scripts/**`, `packages/ui/src/hooks/**`.

These rules are ALWAYS ACTIVE for build-time script execution and client-side interaction hooks using standard runtime console methods.

### Rules

- **R-LOG-001** MUST: Before introducing any external logging dependency to replace built-in console invocations, the consumer MUST inspect the repository lock artifact and verify the exact resolved dependency version against official documentation.
- **R-LOG-002** MUST: Ensure that error messages intended for terminal execution offer actionable next steps to resolve the reported condition.
- **R-LOG-003** MUST: Verify that warning messages in client hooks provide contextual error objects to aid debugging without disrupting user interface state.

### Verify

```bash
# Discover and run the repository linter script from the project manifest
# (Command discovered from repository project manifest)

# Discover and run the project test runner
# (Command discovered from repository project manifest)
```

**Accept when:**
- The discovered project linter completes with zero errors or warnings regarding unauthorized logging calls.
- All automated test suites pass without unhandled diagnostic errors or uncaught logging exceptions.

<enforcement>
Claude Code MUST NOT skip or defer verification. Verified by automated static analysis checks in CI pipelines and peer code review.
</enforcement>