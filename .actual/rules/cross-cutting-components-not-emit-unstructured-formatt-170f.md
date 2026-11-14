---
version: 2
name: cross-cutting-components-not-emit-unstructured-formatt-170f
description: "Structured Context-Aware Logging via log Package: Components Not Emit Unstructured Formatted String"
metadata:
  actual:
    adr_id: 170fe1d8-11bf-492b-b18c-20fa7c160b11
    status: accepted
    candidate_group_id: 39868991-c8ef-4df7-a622-d24edaae0864
    evidence:
      source: deep
  scope:
    root: apps/railway-proxy
---

# Structured Context-Aware Logging via log Package: Components Not Emit Unstructured Formatted String

Scope: applies to `apps/railway-proxy/**`.

These rules are ALWAYS ACTIVE for network transport retry routines, reverse proxy boundary handlers, server lifecycle, and asynchronous process supervisors.

### Rules

- **R-LOG-001** MUST_NOT: Components MUST NOT emit unstructured formatted string errors when structured field binding methods are available in the adopted logging package.

### Verify

```bash
# Discover and run the project's static analysis and linting scripts
# Discover and run the project test suite to validate context propagation and logging execution during transport retries
```

**Accept when:**
- Static analysis passes with zero violations regarding unstructured logging calls in network transport modules.
- All transport and server lifecycle tests execute successfully with verified context-aware log emissions.

<enforcement>
Claude Code MUST NOT skip or defer verification. Verification is mandatory via automated static analysis and peer code review.
</enforcement>