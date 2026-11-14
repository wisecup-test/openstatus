---
version: 2
name: cross-cutting-client-side-user-interface-components-3938
description: "Console API Error and Warning Logging: Client Side User Interface Components Edge"
metadata:
  actual:
    adr_id: 39386bab-d92d-4d51-b63f-0aa82d9bb6c5
    status: accepted
    candidate_group_id: 91834650-166c-4203-af56-53c9938d4f03
    evidence:
      source: deep
  scope:
    root: apps/dashboard
    paths:
      - src/**
---

# Console API Error and Warning Logging: Client Side User Interface Components Edge

Scope: applies to `apps/dashboard/src/**`.

These rules are ALWAYS ACTIVE for all client-side form handlers, user interface event interactions, and routing proxy handlers requiring a lightweight mechanism to report unexpected state transitions and caught promise rejections.

### Rules

- **R-UIE-001** MUST: Client-side user interface components and edge proxy middleware MUST record unexpected exceptions and anomalous application states using the runtime console.error and console.warn interfaces when no centralized logger package is present.

### Verify

```bash
# Discover the project static analysis and linting scripts from the repository configuration and run them to identify unauthorized console usage patterns.
# Discover and execute the automated test suites defined in the repository manifest to verify that error handling blocks behave correctly under failure conditions.
```

**Accept when:**
- Static analysis checks pass with zero unapproved console statements emitting sensitive tokens.
- Automated test suites pass, verifying that error branches execute expected diagnostic reporting paths.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>