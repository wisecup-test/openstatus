---
version: 2
name: cross-cutting-before-integrating-third-party-dependenc-ea92
description: "Recognition Runtime Invocation via recognition.start: Before Integrating Third Party Dependencies Supporting"
metadata:
  actual:
    adr_id: ea92aa97-73a9-43eb-b3f8-ced08f567665
    status: accepted
    candidate_group_id: c15e1737-e236-4335-8e81-899024ca948f
    evidence:
      source: deep
  scope:
    root: apps/dashboard
    paths:
      - src/components/nav/**
---

# Recognition Runtime Invocation via recognition.start: Before Integrating Third Party Dependencies Supporting

Scope: applies to `apps/dashboard/src/components/nav/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-REC-001** MUST: Before integrating third-party dependencies supporting UI interaction or validation routines, consumers MUST inspect the repository lock artifact to determine the exact resolved dependency version.

### Verify

```bash
# Discover and run the repository integration test script from the project manifest
# Execute the project static analysis and linting script to validate component lifecycle hooks
```

**Accept when:**
- All integration test suites covering recognition-dependent components pass in headless environments.
- Recognition lifecycle events trigger expected validation and state mutations without unhandled exceptions.

<enforcement>
Claude Code MUST NOT skip or defer verification. Violation handling: Pull requests containing unmocked hardware API calls that fail in headless CI environments are blocked from merging.
</enforcement>