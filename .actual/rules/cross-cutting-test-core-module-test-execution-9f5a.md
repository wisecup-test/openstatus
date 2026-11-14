---
version: 2
name: cross-cutting-test-core-module-test-execution-9f5a
description: "Adoption of node:test Core Module for Test Execution: Test Utility Implementations Not Rely Unspecified"
metadata:
  actual:
    adr_id: 9f5a6e7b-def0-46fa-9d5e-11c466fbcf6c
    status: accepted
    candidate_group_id: 9ce22998-f17d-41f3-8299-a5e86dfaad71
    evidence:
      source: deep
  scope:
    root: packages/test-utils
    paths:
      - src/**
---

# Adoption of node:test Core Module for Test Execution: Test Utility Implementations Not Rely Unspecified

Scope: applies to `packages/test-utils/src/**`.

These rules are ALWAYS ACTIVE for internal test runner utilities and shared test execution infrastructure.

### Rules

- **R-TEST-001** MUST_NOT: Test utility implementations MUST NOT rely on unspecified third-party test runners within modules designated for node:test execution.

### Verify

```bash
# Find the repository test execution script in the project manifest and execute the test suite
# Verify that test runner utilities successfully invoke test execution through the native testing module without errors
```

**Accept when:**
- The discovered test verification command executes and reports all test suites passing.
- Test execution utilities invoke test lifecycle hooks exclusively through the node:test module.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>