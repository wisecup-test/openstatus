---
version: 2
name: cross-cutting-http-rpc-request-handlers-utilize-afcd
description: "Adoption of @logtape/logtape for Structured Logging and Contextual Wide Events: Http Rpc Request Handlers Utilize Contextual"
metadata:
  actual:
    adr_id: afcdfaa6-f02e-4160-9a75-0dabd95bafd5
    status: accepted
    candidate_group_id: a2c24049-ad05-4492-af5c-05be5a4c42cb
    evidence:
      source: deep
  scope:
    root: apps/server
    paths:
      - src/**
---

# Adoption of @logtape/logtape for Structured Logging and Contextual Wide Events: Http Rpc Request Handlers Utilize Contextual

Scope: applies to `apps/server/src/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-LOG-001** SHOULD: HTTP and RPC request handlers SHOULD utilize contextual execution scopes such as withContext to bind request identifiers and client metadata to all downstream log statements.

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