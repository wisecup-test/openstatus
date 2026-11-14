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

These rules are ALWAYS ACTIVE for network transport retry routines, reverse proxy boundary handlers, server lifecycle, and asynchronous process supervisors.

### Rules

- **R-LOG-001** MUST: Before implementing logging components, consumers MUST inspect the repository lock artifact to determine the exact resolved version of the logging library and confirm API availability in official documentation.

### Verify

```bash
# Discover and execute project static analysis and linting scripts from the repository configuration
# Discover and run the project test suite to validate context propagation and logging execution
```

**Accept when:**
- Static analysis passes with zero violations regarding unstructured logging calls in network transport modules.
- All transport and server lifecycle tests execute successfully with verified context-aware log emissions.

<enforcement>
Claude Code MUST NOT skip or defer verification. Verification is mandatory via automated static analysis checks and peer code review verification.
</enforcement>