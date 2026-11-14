---
version: 2
name: cross-cutting-when-handling-stream-execution-failures-99d5
description: "Stream Controller Error Propagation and console.error Logging: When Handling Stream Execution Failures Error"
metadata:
  actual:
    adr_id: 99d58a0c-d814-45de-9712-7499d33e5b04
    status: accepted
    candidate_group_id: a9337cd3-1360-4529-9190-b2dc06c61223
    evidence:
      source: deep
  scope:
    root: packages/utils
    paths:
      - src/**
---

# Stream Controller Error Propagation and console.error Logging: When Handling Stream Execution Failures Error

Scope: applies to `packages/utils/src/**`.

These rules are ALWAYS ACTIVE for stream processing controllers and pipeline utilities handling asynchronous chunk transformation or source streaming.

### Rules

- **R-STREAM-001** MUST: When handling stream execution failures, stream error handlers MUST log diagnostics via console.error and propagate the error to the stream consumer via controller.error.

### Verify

```bash
# Discover the test runner from the project manifest and execute the test suite covering stream error propagation
# Discover the linter configuration from the project repository and run static analysis checks against stream utility source directories
# Execute the project build command discovered from repository configuration scripts to verify type integrity and error handling signatures
```

**Accept when:**
- Stream utility test cases confirm that controller.error is invoked upon stream failure and downstream readers reject accordingly.
- Static analysis passes without unhandled promise rejections or linting violations in stream controller modules.
- Error scenarios verify that diagnostic logging is emitted to standard error without disrupting stream controller termination.

<enforcement>
Claude Code MUST NOT skip or defer verification. Verified by automated test suites executing stream failure paths and pull request code reviews.
</enforcement>