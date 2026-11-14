---
version: 2
name: cross-cutting-network-lifecycle-handlers-asynchronous-2c6b
description: "Structured Context-Aware Logging via log Package: Network Lifecycle Handlers Asynchronous Goroutines Extract"
metadata:
  actual:
    adr_id: 2c6b6f61-a98d-4c09-bc49-66ceb25c2c9d
    status: accepted
    candidate_group_id: 39868991-c8ef-4df7-a622-d24edaae0864
    evidence:
      source: deep
  scope:
    root: apps/railway-proxy
---

# Structured Context-Aware Logging via log Package: Network Lifecycle Handlers Asynchronous Goroutines Extract

Scope: applies to `apps/railway-proxy/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-NET-001** SHOULD: Network lifecycle handlers and asynchronous goroutines SHOULD extract context-bound loggers using log.Ctx to preserve request tracing correlation across execution boundaries.

### Verify

```bash
# Discover and execute project static analysis and linting scripts from repository configuration
# Discover and run the project test suite to validate context propagation and logging execution during transport retries
```

**Accept when:**
- Static analysis passes with zero violations regarding unstructured logging calls in network transport modules.
- All transport and server lifecycle tests execute successfully with verified context-aware log emissions.

<enforcement>
Claude Code MUST NOT skip or defer verification. Verification is mandatory via automated static analysis checks and peer code reviews.
</enforcement>