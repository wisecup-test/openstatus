---
version: 2
name: cross-cutting-stream-controllers-encountering-unrecove-abdc
description: "Stream Controller Error Propagation and console.error Logging: Stream Controllers Encountering Unrecoverable Faults Notify"
metadata:
  actual:
    adr_id: abdcec8e-d157-45a6-a7da-c16ed44e2f5d
    status: accepted
    candidate_group_id: a9337cd3-1360-4529-9190-b2dc06c61223
    evidence:
      source: deep
  scope:
    root: packages/utils
    paths:
      - src/**
---

# Stream Controller Error Propagation and console.error Logging: Stream Controllers Encountering Unrecoverable Faults Notify

Scope: applies to `packages/utils/src/**`.

These rules are ALWAYS ACTIVE for stream processing controllers and pipeline utilities handling asynchronous chunk transformation or source streaming.

### Rules

- **R-SCM-001** MUST: Stream controllers encountering unrecoverable stream faults MUST notify downstream consumers by invoking controller.error with the captured error instance.

### Verify

```bash
# Discover and execute the test suite covering stream error propagation
# Discover and run static analysis checks against stream utility source directories
# Execute the project build command to verify type integrity and error handling signatures
```

**Accept when:**
- Stream utility test cases confirm that controller.error is invoked upon stream failure and downstream readers reject accordingly.
- Static analysis passes without unhandled promise rejections or linting violations in stream controller modules.
- Error scenarios verify that diagnostic logging is emitted to standard error without disrupting stream controller termination.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>