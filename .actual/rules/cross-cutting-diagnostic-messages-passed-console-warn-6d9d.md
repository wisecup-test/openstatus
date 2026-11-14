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

- **R-DIAG-001** SHOULD: Diagnostic messages passed to console.warn and console.error SHOULD include descriptive invariant context alongside the captured error instance or validation issue structure.
- **R-DIAG-002** MANDATORY: The consumer MUST discover all tool names, file names, commands, package managers, and version numbers from the project repository.
- **R-DIAG-003** MANDATORY: Before writing code that uses a versioned library, execute in order: find dependency manifest, identify build tool, inspect repository lock/resolution artifact, look up official documentation for that exact version, and confirm every API/class/function exists in that version's documentation.
- **R-DIAG-004** MANDATORY: Audit all diagnostic call sites to ensure error objects or string literals do not incorporate sensitive authentication identifiers or private user tokens.
- **R-DIAG-005** MANDATORY: Standardize warning and error string prefixes to simplify filtering within runtime output streams.

### Verify

```bash
# Discover the project static analysis and linting scripts from the repository configuration and run them
# Discover and execute the automated test suites defined in the repository manifest
```

**Accept when:**
- Static analysis checks pass with zero unapproved console statements emitting sensitive tokens.
- Automated test suites pass, verifying that error branches execute expected diagnostic reporting paths.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>