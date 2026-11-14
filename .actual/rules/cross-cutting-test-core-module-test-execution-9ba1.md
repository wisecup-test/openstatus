---
version: 2
name: cross-cutting-test-core-module-test-execution-9ba1
description: "Adoption of node:test Core Module for Test Execution: Developers Inspect Project Resolution Runtime Configuration"
metadata:
  actual:
    adr_id: 9ba11851-37bc-4cbc-807c-4842f7e090ff
    status: accepted
    candidate_group_id: 9ce22998-f17d-41f3-8299-a5e86dfaad71
    evidence:
      source: deep
  scope:
    root: packages/test-utils
    paths:
      - src/**
---

# Adoption of node:test Core Module for Test Execution: Developers Inspect Project Resolution Runtime Configuration

Scope: applies to `packages/test-utils/src/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-NODE-001** MUST: Developers MUST inspect the project resolution and runtime configuration artifacts to verify runtime environment compatibility before authoring suites against the node:test API.

### Verify

```bash
# Find the repository test execution script in the project manifest and execute the test suite.
# Verify that test runner utilities successfully invoke test execution through the native testing module without errors.
```

**Accept when:**
- The discovered test verification command executes and reports all test suites passing.
- Test execution utilities invoke test lifecycle hooks exclusively through the node:test module.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>