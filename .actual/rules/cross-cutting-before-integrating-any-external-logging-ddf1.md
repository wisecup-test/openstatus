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

These rules are ALWAYS ACTIVE for client-side interactive user interface components, form mutation handlers, edge proxy handlers capturing operational anomalies, unhandled promise rejections, and schema parsing discrepancies detected during client runtime execution.

### Rules

- **R-LOG-001** MUST: Before integrating any external logging dependency or observability transport, engineers MUST inspect the repository dependency manifest and lock artifact to determine resolved versions and ecosystem constraints.

### Verify

```bash
# Discover and run the project static analysis, linting, and test scripts from the repository configuration
# Example discovery workflow:
# 1. Read package.json / manifest to locate lint and test scripts
# 2. Run lint check to verify console usage patterns
# 3. Run test suites to verify error handling paths
npm run lint
npm test
```

**Accept when:**
- Static analysis checks pass with zero unapproved console statements emitting sensitive tokens.
- Automated test suites pass, verifying that error branches execute expected diagnostic reporting paths.

<enforcement>
Claude Code MUST NOT skip or defer verification. All pull requests modifying error handling or logging call sites must pass automated static analysis checks and peer code review.
</enforcement>