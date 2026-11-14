---
version: 2
name: cross-cutting-sensitive-credentials-raw-secret-tokens-67c4
description: "Structured Logging with log/slog: Sensitive Credentials Raw Secret Tokens Unmasked"
metadata:
  actual:
    adr_id: 67c41782-251f-4eb7-a8c9-a5d570d85616
    status: accepted
    candidate_group_id: 85876990-3f0b-4766-bd31-5ea7eb5f9942
    evidence:
      source: deep
  scope:
    root: apps/private-location
    paths:
      - internal/server/**
---

# Structured Logging with log/slog: Sensitive Credentials Raw Secret Tokens Unmasked

Scope: applies to `apps/private-location/internal/server/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-LOG-001** MUST_NOT: Sensitive credentials, raw secret tokens, or unmasked authorization headers MUST NOT be passed as attribute values in structured log calls.

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