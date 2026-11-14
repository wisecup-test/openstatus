---
version: 2
name: cross-cutting-automated-integration-test-suites-not-047f
description: "Recognition Runtime Invocation via recognition.start: Automated Integration Test Suites Not Attempt"
metadata:
  actual:
    adr_id: 047f3c97-f556-4ecf-9199-6af2da014e9f
    status: accepted
    candidate_group_id: c15e1737-e236-4335-8e81-899024ca948f
    evidence:
      source: deep
  scope:
    root: apps/dashboard
    paths:
      - src/components/nav/**
---

# Recognition Runtime Invocation via recognition.start: Automated Integration Test Suites Not Attempt

Scope: applies to `apps/dashboard/src/components/nav/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-REC-001** MUST_NOT: Automated integration test suites MUST_NOT attempt direct execution against physical hardware devices in headless continuous integration environments.

### Verify

```bash
# Discover and run the repository integration test script from the project manifest
# to ensure client feedback components pass automated suites without hardware dependencies.
# Execute project static analysis and linting script to validate cleanup lifecycle hooks.
npm test -- --watchAll=false
npm run lint
```

**Accept when:**
- All integration test suites covering recognition-dependent components pass in headless environments.
- Recognition lifecycle events trigger expected validation and state mutations without unhandled exceptions.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>