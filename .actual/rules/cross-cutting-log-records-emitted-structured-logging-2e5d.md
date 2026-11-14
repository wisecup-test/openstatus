---
version: 2
name: cross-cutting-log-records-emitted-structured-logging-2e5d
description: "Structured Logging with log/slog: Log Records Emitted Structured Logging Functions"
metadata:
  actual:
    adr_id: 2e5d736f-b0f3-4958-bd28-41fa30838578
    status: accepted
    candidate_group_id: 85876990-3f0b-4766-bd31-5ea7eb5f9942
    evidence:
      source: deep
  scope:
    root: apps/private-location
    paths:
      - internal/server/**
---

# Structured Logging with log/slog: Log Records Emitted Structured Logging Functions

Scope: applies to `apps/private-location/internal/server/**`.

These rules are ALWAYS ACTIVE for server initialization routines, background workers, and request handler diagnostic logging.

### Rules

- **R-LOG-001** MUST: Log records MUST be emitted using structured logging functions including slog.Info, slog.Warn, slog.Error, or slog.Debug, passing an invariant descriptive message string followed by explicit key-value attribute pairs.

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