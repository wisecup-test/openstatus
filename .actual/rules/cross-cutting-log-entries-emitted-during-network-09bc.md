---
version: 2
name: cross-cutting-log-entries-emitted-during-network-09bc
description: "Structured Context-Aware Logging via log Package: Log Entries Emitted During Network Transport"
metadata:
  actual:
    adr_id: 09bcd4d1-becf-49f0-b790-1b5bc57758bf
    status: accepted
    candidate_group_id: 39868991-c8ef-4df7-a622-d24edaae0864
    evidence:
      source: deep
  scope:
    root: apps/railway-proxy
---

# Structured Context-Aware Logging via log Package: Log Entries Emitted During Network Transport

Scope: applies to `apps/railway-proxy/**`.

These rules are ALWAYS ACTIVE for network transport retry routines, reverse proxy boundary handlers, server lifecycle, and asynchronous process supervisors.

### Rules

- **R-NET-001** MUST: Log entries emitted during network transport execution and failure retries MUST attach structured attributes for URL, attempt index, and the captured error via chained builder calls before invoking the message emission method.

### Verify

```bash
# Discover and execute the project's static analysis and linting scripts from the repository configuration
# Discover and run the project test suite to validate context propagation and logging execution during transport retries
```

**Accept when:**
- Static analysis passes with zero violations regarding unstructured logging calls in network transport modules.
- All transport and server lifecycle tests execute successfully with verified context-aware log emissions.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>