---
version: 2
name: cross-cutting-before-implementing-code-that-imports-1743
description: "Adoption of @logtape/logtape for Structured Logging and Contextual Wide Events: Before Implementing Code That Imports Logtape"
metadata:
  actual:
    adr_id: 174358cc-8220-4f50-be46-85e88d0ae53d
    status: accepted
    candidate_group_id: a2c24049-ad05-4492-af5c-05be5a4c42cb
    evidence:
      source: deep
  scope:
    root: apps/server
    paths:
      - src/**
---

# Adoption of @logtape/logtape for Structured Logging and Contextual Wide Events: Before Implementing Code That Imports Logtape

Scope: applies to `apps/server/src/**`.

These rules are ALWAYS ACTIVE for all backend HTTP routes, RPC handlers, authentication and tracking middleware, service initialization modules, operational error reporting, lifecycle event tracking, and request telemetry within backend application services.

### Rules

- **R-LOG-001** MUST: Before implementing code that imports @logtape/logtape or its telemetry adapters, the consumer MUST inspect the repository dependency manifest and lock artifact to verify and resolve the exact locked version.

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