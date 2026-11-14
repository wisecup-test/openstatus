---
version: 2
name: cross-cutting-log-entries-include-domain-specific-535e
description: "Structured Logging with log/slog: Log Entries Include Domain Specific Context"
metadata:
  actual:
    adr_id: 535ed417-9818-41fd-8a2c-f8ac21a499b4
    status: accepted
    candidate_group_id: 85876990-3f0b-4766-bd31-5ea7eb5f9942
    evidence:
      source: deep
  scope:
    root: apps/private-location
    paths:
      - internal/server/**
---

# Structured Logging with log/slog: Log Entries Include Domain Specific Context

Scope: applies to `apps/private-location/internal/server/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-LOG-001** SHOULD: Log entries SHOULD include domain-specific context attributes such as monitor identifiers and location identifiers when logging events within request handlers or background jobs.

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