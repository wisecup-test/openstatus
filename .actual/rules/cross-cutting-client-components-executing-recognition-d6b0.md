---
version: 2
name: cross-cutting-client-components-executing-recognition-d6b0
description: "Recognition Runtime Invocation via recognition.start: Client Components Executing Recognition Start Routines"
metadata:
  actual:
    adr_id: d6b04499-1aaa-4f4a-a7e4-141b8f3c65b8
    status: accepted
    candidate_group_id: c15e1737-e236-4335-8e81-899024ca948f
    evidence:
      source: deep
  scope:
    root: apps/dashboard
    paths:
      - src/components/nav/**
---

# Recognition Runtime Invocation via recognition.start: Client Components Executing Recognition Start Routines

Scope: applies to `apps/dashboard/src/components/nav/**`.

These rules are ALWAYS ACTIVE for interactive client-side components utilizing recognition.start routines and integration test suites targeting user interface feedback modules.

### Rules

- **R-REC-001** MUST: Client components executing recognition.start routines MUST encapsulate runtime hardware calls behind interface boundaries to allow substitution during integration testing.

### Verify

```bash
# Discover and run the repository integration test script from the project manifest to ensure client feedback components pass automated suites without hardware dependencies.
# Execute the project static analysis and linting script to validate that component lifecycle hooks correctly manage cleanup for active recognition sessions.
```

**Accept when:**
- All integration test suites covering recognition-dependent components pass in headless environments.
- Recognition lifecycle events trigger expected validation and state mutations without unhandled exceptions.

<enforcement>
Claude Code MUST NOT skip or defer verification. Verification is mandatory via continuous integration automated test suite execution on pull requests and peer architectural code review.
</enforcement>