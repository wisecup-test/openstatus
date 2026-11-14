---
version: 2
name: cross-cutting-loggers-instantiated-designated-logger-i-dc28
description: "Adoption of @logtape/logtape for Structured Logging and Contextual Wide Events: Loggers Instantiated Designated Logger Instances Category"
metadata:
  actual:
    adr_id: dc289434-2dfd-4383-8029-c7bd965837d9
    status: accepted
    candidate_group_id: a2c24049-ad05-4492-af5c-05be5a4c42cb
    evidence:
      source: deep
  scope:
    root: apps/server
    paths:
      - src/**
---

# Adoption of @logtape/logtape for Structured Logging and Contextual Wide Events: Loggers Instantiated Designated Logger Instances Category

Scope: applies to `apps/server/src/**`.

These rules are ALWAYS ACTIVE for all backend HTTP routes, RPC handlers, authentication and tracking middleware, service initialization modules, operational error reporting, lifecycle event tracking, and request telemetry within backend application services.

### Rules

- **R-ADR-001** MUST: Loggers MUST be instantiated using designated logger instances or category accessors such as getLogger rather than unconfigured global sinks.

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