---
version: 2
name: cross-cutting-applications-utilizing-log-package-use-fc7f
description: "Structured Context-Aware Logging via log Package: Applications Utilizing Log Package Use Structured"
metadata:
  actual:
    adr_id: fc7f6ccd-0c9e-4042-a0d0-8e4cba18b462
    status: accepted
    candidate_group_id: 39868991-c8ef-4df7-a622-d24edaae0864
    evidence:
      source: deep
  scope:
    root: apps/railway-proxy
---

# Structured Context-Aware Logging via log Package: Applications Utilizing Log Package Use Structured

Scope: applies to `apps/railway-proxy/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-LOG-001** MUST: Applications utilizing the log package MUST use structured logging methods including log.Ctx for context-bound logging and leveled loggers with explicit field chaining for diagnostic telemetry.

### Verify

```bash
# Discover and execute the project's static analysis and linting scripts from the repository configuration
# Discover and run the project test suite to validate context propagation and logging execution during transport retries
```

**Accept when:**
- Static analysis passes with zero violations regarding unstructured logging calls in network transport modules.
- All transport and server lifecycle tests execute successfully with verified context-aware log emissions.

<enforcement>
Claude Code MUST NOT skip or defer verification. Verification is mandatory via automated static analysis and peer code review.
</enforcement>