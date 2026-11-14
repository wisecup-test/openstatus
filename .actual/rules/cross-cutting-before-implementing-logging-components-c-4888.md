---
version: 2
name: cross-cutting-before-implementing-logging-components-c-4888
description: "Structured Context-Aware Logging via log Package: Before Implementing Logging Components Consumers Inspect"
metadata:
  actual:
    adr_id: 4888682d-6f90-4d29-9751-36db026761e3
    status: accepted
    candidate_group_id: 39868991-c8ef-4df7-a622-d24edaae0864
    evidence:
      source: deep
  scope:
    root: apps/railway-proxy
---

# Structured Context-Aware Logging via log Package: Before Implementing Logging Components Consumers Inspect

Scope: applies to `apps/railway-proxy/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-LOG-001** MUST: Before implementing logging components, consumers MUST inspect the repository lock artifact to determine the exact resolved version of the logging library and confirm API availability in official documentation.
- **R-LOG-002** MUST: Instantiate structured log chains at the point of failure before scheduling backoff delays when constructing retry loops for transport handlers.
- **R-LOG-003** MUST: Extract the logger from the request context using log.Ctx to ensure tracing identifiers are automatically attached to outgoing log messages.

### Verify

```bash
# Discover and execute project static analysis and linting scripts from the repository configuration
# Discover and run the project test suite to validate context propagation and logging execution during transport retries
```

**Accept when:**
- Static analysis passes with zero violations regarding unstructured logging calls in network transport modules.
- All transport and server lifecycle tests execute successfully with verified context-aware log emissions.

<enforcement>
Claude Code MUST NOT skip or defer verification. Verification is mandatory via automated static analysis and peer code review.
</enforcement>