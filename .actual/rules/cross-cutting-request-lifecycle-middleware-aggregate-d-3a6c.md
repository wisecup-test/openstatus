---
version: 2
name: cross-cutting-request-lifecycle-middleware-aggregate-d-3a6c
description: "Adoption of @logtape/logtape for Structured Logging and Contextual Wide Events: Request Lifecycle Middleware Aggregate Duration Status"
metadata:
  actual:
    adr_id: 3a6cd9a3-4060-4a32-a59d-847de189554a
    status: accepted
    candidate_group_id: a2c24049-ad05-4492-af5c-05be5a4c42cb
    evidence:
      source: deep
  scope:
    root: apps/server
    paths:
      - src/**
---

# Adoption of @logtape/logtape for Structured Logging and Contextual Wide Events: Request Lifecycle Middleware Aggregate Duration Status

Scope: applies to `apps/server/src/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-LOG-001** SHOULD: Request lifecycle middleware SHOULD aggregate duration, status codes, and error outcomes into a single canonical wide event log entry upon request completion.

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