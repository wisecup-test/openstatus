---
version: 2
name: cross-cutting-service-application-adopt-logtape-standa-9832
description: "Adoption of @logtape/logtape for Structured Logging and Contextual Wide Events: Service Application Adopt Logtape Standard Logging"
metadata:
  actual:
    adr_id: 9832cf37-400b-43dd-9635-0e374ddeb3f7
    status: accepted
    candidate_group_id: a2c24049-ad05-4492-af5c-05be5a4c42cb
    evidence:
      source: deep
  scope:
    root: apps/server
    paths:
      - src/**
---

# Adoption of @logtape/logtape for Structured Logging and Contextual Wide Events: Service Application Adopt Logtape Standard Logging

Scope: applies to `apps/server/src/**`.

These rules are ALWAYS ACTIVE for all backend service layers, route handlers, middleware, RPC interceptors, operational error reporting, lifecycle event tracking, and request telemetry within backend application services.

### Rules

- **R-LOG-001** MUST: The service application MUST adopt @logtape/logtape as the standard logging framework for all backend service layers, route handlers, middleware, and RPC interceptors.

### Verify

```bash
# Discover and execute repository static analysis and linting scripts to verify absence of unapproved direct console logging across server source files
# Discover and run project test suite to verify structured log formatting and contextual log propagation
```

**Accept when:**
- Static analysis and linting verification checks pass with zero unapproved console logging invocations in server modules.
- All server handlers, middleware, and interceptors emit log events using the designated logging framework with structured object metadata payloads.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>