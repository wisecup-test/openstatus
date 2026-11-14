---
version: 2
name: cross-cutting-test-core-module-test-execution-d86a
description: "Adoption of node:test Core Module for Test Execution: Test Runner Utilities Utilizing Native Orchestration"
metadata:
  actual:
    adr_id: d86acda1-657f-4014-81f0-6ed3a78a7b38
    status: accepted
    candidate_group_id: 9ce22998-f17d-41f3-8299-a5e86dfaad71
    evidence:
      source: deep
  scope:
    root: packages/test-utils
    paths:
      - src/**
---

# Adoption of node:test Core Module for Test Execution: Test Runner Utilities Utilizing Native Orchestration

Scope: applies to `packages/test-utils/src/**`.

These rules are ALWAYS ACTIVE for internal test runner utilities and shared test execution infrastructure.

### Rules

- **R-TEST-001** MUST: Test runner utilities utilizing native test orchestration MUST import and execute test suites via the node:test core module.

### Verify

```bash
# Find the repository test execution script in the project manifest and execute the test suite.
# (Discover test execution scripts from configuration and verify native testing module execution)
```

**Accept when:**
- The discovered test verification command executes and reports all test suites passing.
- Test execution utilities invoke test lifecycle hooks exclusively through the node:test module.

<enforcement>
Claude Code MUST NOT skip or defer verification. Verification is mandatory via continuous integration automated test suite execution and code review.
</enforcement>