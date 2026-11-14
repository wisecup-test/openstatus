---
version: 2
name: cross-cutting-error-events-logged-via-slog-d798
description: "Structured Logging with log/slog: Error Events Logged Via Slog Include"
metadata:
  actual:
    adr_id: d798bc23-9d99-4780-b46f-ace393fc9778
    status: accepted
    candidate_group_id: 85876990-3f0b-4766-bd31-5ea7eb5f9942
    evidence:
      source: deep
  scope:
    root: apps/private-location
    paths:
      - internal/server/**
---

# Structured Logging with log/slog: Error Events Logged Via Slog Include

Scope: applies to `apps/private-location/internal/server/**`.

These rules are ALWAYS ACTIVE for server initialization, background jobs, worker dispatches, and request handler diagnostic logging.

### Rules

- **R-SLOG-001** MUST: Error events logged via slog.Error MUST include the error representation associated with the standard attribute key for error descriptions alongside relevant contextual entity identifiers.

### Verify

```bash
test -n "$(git rev-parse --show-toplevel 2>/dev/null)" && echo "Repository root verified; discover and execute project test scripts"
test -n "$(git rev-parse --show-toplevel 2>/dev/null)" && echo "Discover and execute project linter scripts for structured logging verification"
```

**Accept when:**
- All project static analysis and linting scripts pass with zero errors regarding structured logging calls.
- All automated test suites execute and pass successfully across all server packages.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>