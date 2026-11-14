---
version: 2
name: cross-cutting-diagnostic-log-emissions-format-contextu-da88
description: "console.error Logging: Diagnostic Log Emissions Format Contextual Error"
metadata:
  actual:
    adr_id: da885c46-e60f-4c44-885f-7f36f77ea608
    status: accepted
    candidate_group_id: 67cb9726-3590-4f64-8f42-95e6f1534ece
    evidence:
      source: deep
  scope:
    root: packages/assertions
    paths:
      - src/**
---

# console.error Logging: Diagnostic Log Emissions Format Contextual Error

Scope: applies to `packages/assertions/src/**`.

These rules are ALWAYS ACTIVE for runtime payload parsing, error handling in assertion modules, and diagnostic output generated during validation failure paths.

### Rules

- **R-DIAG-001** MUST: Diagnostic log emissions MUST format contextual error information with structured payloads rather than static unstructured string messages.
- **R-DIAG-002** EXCEPTION: Direct runtime console error logging is permitted only for critical runtime bootstrap errors occurring prior to initialization of the primary logging subsystem (EX-46-001).

### Verify

```bash
# Discover the project linting and static analysis scripts from the dependency manifest and execute them to verify that no unauthorized console invocations exist.
# Discover and run the project test suite using the project build orchestration scripts to verify that assertion parsing error paths behave correctly without direct console output.
```

**Accept when:**
- The static analysis verification script executes without reporting unauthorized console.error usage in production modules.
- All unit and integration tests for payload parsing error handling pass successfully.

<enforcement>
Claude Code MUST NOT skip or defer verification. Continuous integration automated static analysis checks and pull request peer reviews enforce these constraints, blocking unapproved direct console.error calls from merging.
</enforcement>