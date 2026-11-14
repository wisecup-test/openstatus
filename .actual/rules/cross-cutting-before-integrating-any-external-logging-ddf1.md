---
version: 2
name: cross-cutting-before-integrating-any-external-logging-ddf1
description: "Console API Error and Warning Logging: Before Integrating Any External Logging Dependency"
metadata:
  actual:
    adr_id: ddf176af-613e-4e5b-ba07-0d7515c1cd4b
    status: accepted
    candidate_group_id: 91834650-166c-4203-af56-53c9938d4f03
    evidence:
      source: deep
  scope:
    root: apps/dashboard
    paths:
      - src/**
---

# Console API Error and Warning Logging: Before Integrating Any External Logging Dependency

Scope: applies to `apps/dashboard/src/**`.

These rules are ALWAYS ACTIVE for client-side interactive user interface components, form mutation handlers, edge proxy handlers, unhandled promise rejections, and schema parsing discrepancies.

### Rules

- **R-LOG-001** MUST: Before integrating any external logging dependency or observability transport, engineers MUST inspect the repository dependency manifest and lock artifact to determine resolved versions and ecosystem constraints.

### Verify

```bash
# Discover the project static analysis and linting scripts from the repository configuration and run them
# Example discovery / execution pattern:
# npm run lint
# npm test
```

**Accept when:**
- Static analysis checks pass with zero unapproved console statements emitting sensitive tokens.
- Automated test suites pass, verifying that error branches execute expected diagnostic reporting paths.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>