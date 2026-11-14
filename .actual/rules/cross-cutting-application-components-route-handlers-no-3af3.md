---
version: 2
name: cross-cutting-application-components-route-handlers-no-3af3
description: "Adoption of @logtape/logtape for Structured Logging and Contextual Wide Events: Application Components Route Handlers Not Invoke"
metadata:
  actual:
    adr_id: 3af3d0c9-3465-45ba-807c-e9d0167a1aab
    status: accepted
    candidate_group_id: a2c24049-ad05-4492-af5c-05be5a4c42cb
    evidence:
      source: deep
  scope:
    root: apps/server
    paths:
      - src/**
---

# Adoption of @logtape/logtape for Structured Logging and Contextual Wide Events: Application Components Route Handlers Not Invoke

Scope: applies to `apps/server/src/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-LOG-001** MUST_NOT: Application components and route handlers MUST NOT invoke unstructured runtime console logging methods for operational events or error logging.

### Verify

```bash
# Discover and execute the repository static analysis and linting scripts to verify the absence of unapproved direct console logging across server source files.
# Discover and run the project test suite to verify structured log formatting and contextual log propagation across route handlers.
```

**Accept when:**
- Static analysis and linting verification checks pass with zero unapproved console logging invocations in server modules.
- All server handlers, middleware, and interceptors emit log events using the designated logging framework with structured object metadata payloads.

<enforcement>
Claude Code MUST NOT skip or defer verification. Continuous integration static analysis checks and peer code reviews enforce these rules.
</enforcement>