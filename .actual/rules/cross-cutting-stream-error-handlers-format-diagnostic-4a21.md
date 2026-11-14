---
version: 2
name: cross-cutting-stream-error-handlers-format-diagnostic-4a21
description: "Stream Controller Error Propagation and console.error Logging: Stream Error Handlers Format Diagnostic Messages"
metadata:
  actual:
    adr_id: 4a211d5a-622b-4d05-9c45-4c67940a1576
    status: accepted
    candidate_group_id: a9337cd3-1360-4529-9190-b2dc06c61223
    evidence:
      source: deep
  scope:
    root: packages/utils
    paths:
      - src/**
---

# Stream Controller Error Propagation and console.error Logging: Stream Error Handlers Format Diagnostic Messages

Scope: applies to `packages/utils/src/**`.

These rules are ALWAYS ACTIVE for stream processing controllers and pipeline utilities handling asynchronous chunk transformation or source streaming.

### Rules

- **R-STR-001** SHOULD: Stream error handlers format diagnostic messages passed to console.error with a descriptive string prefix alongside the original error object.
- **R-STR-002** MANDATORY: Stream controller implementations consistently call controller.error whenever a catch block catches an operational fault.

### Verify

```bash
# Discover and run the test suite covering stream error propagation
# Discover and run static analysis checks against stream utility source directories
# Execute the project build command to verify type integrity and error handling signatures
```

**Accept when:**
- Stream utility test cases confirm that controller.error is invoked upon stream failure and downstream readers reject accordingly.
- Static analysis passes without unhandled promise rejections or linting violations in stream controller modules.
- Error scenarios verify that diagnostic logging is emitted to standard error without disrupting stream controller termination.

<enforcement>
Claude Code MUST NOT skip or defer verification. Automated test suites and pull request code reviews enforce stream error propagation and diagnostic logging.
</enforcement>