---
version: 2
name: cross-cutting-modules-not-invoke-global-console-ef89
description: "console.error Logging: Modules Not Invoke Global Console Error"
metadata:
  actual:
    adr_id: ef8980ab-ac8b-477e-823f-550882b82ee1
    status: accepted
    candidate_group_id: 67cb9726-3590-4f64-8f42-95e6f1534ece
    evidence:
      source: deep
  scope:
    root: packages/assertions
    paths:
      - src/**
---

# console.error Logging: Modules Not Invoke Global Console Error

Scope: applies to `packages/assertions/src/**`.

These rules are ALWAYS ACTIVE for runtime payload parsing, error handling in assertion modules, and diagnostic output generated during validation failure paths.

### Rules

- **R-46-001** MUST_NOT: Modules MUST NOT invoke global console.error directly for runtime error reporting in production execution paths without routing through an approved structured logging interface.

### Verify

```bash
# Discover and execute project linting / static analysis scripts from the dependency manifest
# Example: npm run lint / yarn lint

# Discover and run the project test suite using build orchestration scripts
# Example: npm test / yarn test
```

**Accept when:**
- The static analysis verification script executes without reporting unauthorized console.error usage in production modules.
- All unit and integration tests for payload parsing error handling pass successfully.

<enforcement>
Claude Code MUST NOT skip or defer verification. Continuous integration automated static analysis checks and pull request peer reviews enforce these rules.
</enforcement>