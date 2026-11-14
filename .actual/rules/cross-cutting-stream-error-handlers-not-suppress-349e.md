---
version: 2
name: cross-cutting-stream-error-handlers-not-suppress-349e
description: "Stream Controller Error Propagation and console.error Logging: Stream Error Handlers Not Suppress Swallow"
metadata:
  actual:
    adr_id: 349ea7b7-824b-46d6-abc0-fdbfd139db70
    status: accepted
    candidate_group_id: a9337cd3-1360-4529-9190-b2dc06c61223
    evidence:
      source: deep
  scope:
    root: packages/utils
    paths:
      - src/**
---

# Stream Controller Error Propagation and console.error Logging: Stream Error Handlers Not Suppress Swallow

Scope: applies to `packages/utils/src/**`.

These rules are ALWAYS ACTIVE for stream processing controllers and pipeline utilities handling asynchronous chunk transformation or source streaming.

### Rules

- **R-STREAM-001** MUST_NOT: Stream error handlers MUST NOT suppress or swallow error instances without propagating the termination signal to controller.error.

### Verify

```bash
# Discover test runner and run test suite covering stream error propagation
# Discover linter config and run static analysis checks against stream utility source directories
# Execute project build command to verify type integrity and error handling signatures
```

**Accept when:**
- Stream utility test cases confirm that controller.error is invoked upon stream failure and downstream readers reject accordingly.
- Static analysis passes without unhandled promise rejections or linting violations in stream controller modules.
- Error scenarios verify that diagnostic logging is emitted to standard error without disrupting stream controller termination.

<enforcement>
Claude Code MUST NOT skip or defer verification. Verified by automated test suites, pull request code reviews, and blocking non-compliant pull requests.
</enforcement>