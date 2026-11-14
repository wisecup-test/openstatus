---
version: 2
name: cross-cutting-server-codebase-adopt-standard-library-8451
description: "Structured Logging with log/slog: Server Codebase Adopt Standard Library Structured"
metadata:
  actual:
    adr_id: 8451f3d0-438a-405e-acfe-cd329b9bea28
    status: accepted
    candidate_group_id: 85876990-3f0b-4766-bd31-5ea7eb5f9942
    evidence:
      source: deep
  scope:
    root: apps/private-location
    paths:
      - internal/server/**
---

# Structured Logging with log/slog: Server Codebase Adopt Standard Library Structured

Scope: applies to `apps/private-location/internal/server/**`.

These rules are ALWAYS ACTIVE for all server code files matching the configured scope.

### Rules

- **R-LOG-001** MUST: The server codebase MUST adopt the standard library structured logging package log/slog as the primary logging mechanism for all server lifecycle events, operational state notifications, and failure reporting.

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