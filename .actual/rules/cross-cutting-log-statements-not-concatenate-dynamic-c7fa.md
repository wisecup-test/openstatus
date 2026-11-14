---
version: 2
name: cross-cutting-log-statements-not-concatenate-dynamic-c7fa
description: "Structured Logging with log/slog: Log Statements Not Concatenate Dynamic Variables"
metadata:
  actual:
    adr_id: c7fa3b7c-74f9-44ab-8b95-d353ee2621c2
    status: accepted
    candidate_group_id: 85876990-3f0b-4766-bd31-5ea7eb5f9942
    evidence:
      source: deep
  scope:
    root: apps/private-location
    paths:
      - internal/server/**
---

# Structured Logging with log/slog: Log Statements Not Concatenate Dynamic Variables

Scope: applies to `apps/private-location/internal/server/**`.

These rules are ALWAYS ACTIVE for server initialization and lifecycle event logging, background asynchronous job and worker dispatch failure reporting, and HTTP and RPC server request handler diagnostic logging.

### Rules

- **R-LOG-001** MUST_NOT: Log statements MUST NOT concatenate dynamic variables or formatted values directly into the message argument string.

### Verify

```bash
test -n "$(git rev-parse --show-toplevel 2>/dev/null)" && echo "Repository root verified; discover and execute project test scripts"
test -n "$(git rev-parse --show-toplevel 2>/dev/null)" && echo "Discover and execute project linter scripts for structured logging verification"
```

**Accept when:**
- All project static analysis and linting scripts pass with zero errors regarding structured logging calls.
- All automated test suites execute and pass successfully across all server packages.

<enforcement>
Claude Code MUST NOT skip or defer verification. Continuous integration and pull request peer reviews enforce structured logging conventions and balanced attribute key-value pairs.
</enforcement>