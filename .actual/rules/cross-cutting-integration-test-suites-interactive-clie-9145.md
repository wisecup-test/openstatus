---
version: 2
name: cross-cutting-integration-test-suites-interactive-clie-9145
description: "Recognition Runtime Invocation via recognition.start: Integration Test Suites Interactive Client Components"
metadata:
  actual:
    adr_id: 914505ba-cc63-4cfa-9a53-58ada3651884
    status: accepted
    candidate_group_id: c15e1737-e236-4335-8e81-899024ca948f
    evidence:
      source: deep
  scope:
    root: apps/dashboard
    paths:
      - src/components/nav/**
---

# Recognition Runtime Invocation via recognition.start: Integration Test Suites Interactive Client Components

Scope: applies to `apps/dashboard/src/components/nav/**`.

These rules are ALWAYS ACTIVE for all interactive client-side components utilizing recognition.start routines and integration test suites targeting user interface feedback modules.

### Rules

- **R-REC-001** SHOULD: Integration test suites for interactive client components SHOULD verify speech recognition initialization, success, and error handling states using deterministic test doubles.

### Verify

```bash
# Discover and run the repository integration test script from the project manifest
# Execute the project static analysis and linting script to validate cleanup hooks
```

**Accept when:**
- All integration test suites covering recognition-dependent components pass in headless environments.
- Recognition lifecycle events trigger expected validation and state mutations without unhandled exceptions.

<enforcement>
Verification is mandatory. Claude Code MUST NOT skip or defer verification.
</enforcement>