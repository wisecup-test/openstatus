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

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-DIAG-001** MUST: Diagnostic log emissions MUST format contextual error information with structured payloads rather than static unstructured string messages.

### Verify

```bash
# Discover and execute project linting/static analysis scripts to check for unauthorized console invocations
# Discover and run the project test suite using build orchestration scripts
```

**Accept when:**
- The static analysis verification script executes without reporting unauthorized console.error usage in production modules.
- All unit and integration tests for payload parsing error handling pass successfully.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>