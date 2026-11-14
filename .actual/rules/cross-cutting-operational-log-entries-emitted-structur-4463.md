---
version: 2
name: cross-cutting-operational-log-entries-emitted-structur-4463
description: "Adoption of @logtape/logtape for Structured Logging and Contextual Wide Events: Operational Log Entries Emitted Structured Property"
metadata:
  actual:
    adr_id: 44634012-51fb-4f9e-b4d7-7ef7a0219472
    status: accepted
    candidate_group_id: a2c24049-ad05-4492-af5c-05be5a4c42cb
    evidence:
      source: deep
  scope:
    root: apps/server
    paths:
      - src/**
---

# Adoption of @logtape/logtape for Structured Logging and Contextual Wide Events: Operational Log Entries Emitted Structured Property

Scope: applies to `apps/server/src/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-LOG-001** MUST: All operational log entries MUST be emitted using structured property payloads as key-value objects rather than string interpolation or string concatenation.

### Verify

```bash
# Discover and execute the repository static analysis and linting scripts to verify the absence of unapproved direct console logging across server source files.
# Discover and run the project test suite to verify structured log formatting and contextual log propagation across route handlers.
```

**Accept when:**
- Static analysis and linting verification checks pass with zero unapproved console logging invocations in server modules.
- All server handlers, middleware, and interceptors emit log events using the designated logging framework with structured object metadata payloads.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>