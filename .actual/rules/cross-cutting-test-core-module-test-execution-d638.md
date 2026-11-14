---
version: 2
name: cross-cutting-test-core-module-test-execution-d638
description: "Adoption of node:test Core Module for Test Execution: Developers Not Introduce External Testing Framework"
metadata:
  actual:
    adr_id: d638bcad-c255-43ae-9b95-b7b017732e1d
    status: accepted
    candidate_group_id: 9ce22998-f17d-41f3-8299-a5e86dfaad71
    evidence:
      source: deep
  scope:
    root: packages/test-utils
    paths:
      - src/**
---

# Adoption of node:test Core Module for Test Execution: Developers Not Introduce External Testing Framework

Scope: applies to `packages/test-utils/src/**`.

These rules are ALWAYS ACTIVE for internal test runner utilities and shared test execution infrastructure.

### Rules

- **R-TEST-001** SHOULD NOT: Developers SHOULD NOT introduce external testing framework runners into internal test execution utilities when native capabilities provided by node:test satisfy requirements.

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