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

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-STREAM-001** MUST: Stream controllers encountering unrecoverable stream faults MUST notify downstream consumers by invoking controller.error with the captured error instance.

### Verify

```bash
# Discover test runner from project manifest and execute stream error propagation tests
test_runner=$(npm pkg get scripts.test 2>/dev/null || echo "npm test")
if [ -n "$test_runner" ]; then
  eval $(npm pkg get scripts.test | tr -d '"')
fi

# Run linter checks
lint_runner=$(npm pkg get scripts.lint 2>/dev/null)
if [ -n "$lint_runner" ]; then
  eval $(npm pkg get scripts.lint | tr -d '"')
fi

# Execute project build command
build_runner=$(npm pkg get scripts.build 2>/dev/null)
if [ -n "$build_runner" ]; then
  eval $(npm pkg get scripts.build | tr -d '"')
fi
```

**Accept when:**
- Stream utility test cases confirm that controller.error is invoked upon stream failure and downstream readers reject accordingly.
- Static analysis passes without unhandled promise rejections or linting violations in stream controller modules.
- Error scenarios verify that diagnostic logging is emitted to standard error without disrupting stream controller termination.

<enforcement>
Claude Code MUST NOT skip or defer verification. All pull requests omitting error propagation or diagnostic logging in stream handlers are blocked until addressed.
</enforcement>