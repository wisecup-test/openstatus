---
version: 2
name: cross-cutting-components-not-use-informational-debug-ffc4
description: "Console API Error and Warning Logging: Components Not Use Informational Debug Console"
metadata:
  actual:
    adr_id: ffc4e72a-f4a1-4cc6-b1fb-1f53f9e328f8
    status: accepted
    candidate_group_id: 91834650-166c-4203-af56-53c9938d4f03
    evidence:
      source: deep
  scope:
    root: apps/dashboard
    paths:
      - src/**
---

# Console API Error and Warning Logging: Components Not Use Informational Debug Console

Scope: applies to `apps/dashboard/src/**`.

These rules are ALWAYS ACTIVE for client-side interactive user interface components, form mutation handlers, and edge proxy handlers capturing operational anomalies, as well as unhandled promise rejections and schema parsing discrepancies detected during client runtime execution.

### Rules

- **R-LOG-001** SHOULD_NOT: Components SHOULD_NOT use informational or debug console methods in production execution paths where error handling interfaces suffice.

### Verify

```bash
# Discover and run project static analysis/linting scripts to identify unauthorized console usage patterns
# Discover and execute automated test suites defined in the repository manifest
```

**Accept when:**
- Static analysis checks pass with zero unapproved console statements emitting sensitive tokens.
- Automated test suites pass, verifying that error branches execute expected diagnostic reporting paths.

<enforcement>
Claude Code MUST NOT skip or defer verification. All pull requests must be verified via automated static analysis and peer code review.
</enforcement>