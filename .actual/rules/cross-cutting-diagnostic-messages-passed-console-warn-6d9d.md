---
version: 2
name: cross-cutting-diagnostic-messages-passed-console-warn-6d9d
description: "Console API Error and Warning Logging: Diagnostic Messages Passed Console Warn Error"
metadata:
  actual:
    adr_id: 6d9d78e4-4a4b-4089-b82d-00cf000c3966
    status: accepted
    candidate_group_id: 91834650-166c-4203-af56-53c9938d4f03
    evidence:
      source: deep
  scope:
    root: apps/dashboard
    paths:
      - src/**
---

# Console API Error and Warning Logging: Diagnostic Messages Passed Console Warn Error

Scope: applies to `apps/dashboard/src/**`.

These rules are ALWAYS ACTIVE for client-side interactive user interface components, form mutation handlers, edge proxy handlers, unhandled promise rejections, and schema parsing discrepancies detected during client runtime execution.

### Rules

- **R-LOG-001** SHOULD: Diagnostic messages passed to console.warn and console.error SHOULD include descriptive invariant context alongside the captured error instance or validation issue structure.
- **R-LOG-002** MANDATORY: Audit all diagnostic call sites to ensure error objects or string literals do not incorporate sensitive authentication identifiers or private user tokens.
- **R-LOG-003** MANDATORY: Standardize warning and error string prefixes to simplify filtering within runtime output streams.

### Verify

```bash
# Discover and run the project static analysis and linting scripts from the repository configuration
# Example: npm run lint / yarn lint

# Discover and execute the automated test suites defined in the repository manifest
# Example: npm test / yarn test
```

**Accept when:**
- Static analysis checks pass with zero unapproved console statements emitting sensitive tokens.
- Automated test suites pass, verifying that error branches execute expected diagnostic reporting paths.

<enforcement>
Claude Code MUST NOT skip or defer verification. Automated static analysis and test suites must be executed to verify logging call sites.
</enforcement>