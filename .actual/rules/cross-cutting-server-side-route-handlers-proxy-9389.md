---
version: 2
name: cross-cutting-server-side-route-handlers-proxy-9389
description: "Adoption of next/server for Server Route and Request Handling: Server Side Route Handlers Proxy Request"
metadata:
  actual:
    adr_id: 9389a306-45e9-4329-904f-e7b780cecf3a
    status: accepted
    candidate_group_id: 2dd81b33-c7f2-44a9-87ad-46fa4c700394
    evidence:
      source: deep
  scope:
    root: apps/dashboard
    paths:
      - src/**
---

# Adoption of next/server for Server Route and Request Handling: Server Side Route Handlers Proxy Request

Scope: applies to `apps/dashboard/src/**`.

These rules are ALWAYS ACTIVE for all server-side HTTP route handlers, edge and server proxy dispatchers, webhook receivers, and identity provider callback endpoints.

### Rules

- **R-SVR-001** MUST: All server-side route handlers and proxy request processors MUST use the next/server module to handle HTTP request inspection, cookie mutation, and response generation.

### Verify

```bash
# Discover the project test runner from repository configuration and execute the server test suite.
# Discover the project type checker from repository configuration and verify type compliance across server route handlers.
# Discover the project linter from repository configuration and execute static analysis across server modules.
```

**Accept when:**
- All server route handlers and proxy modules compile without type errors using the server module interfaces.
- HTTP endpoint test suites pass successfully, verifying header extraction and cookie lifecycle management.
- Static analysis confirms no unauthorized direct transport mutations or unsupported runtime socket access in server route handlers.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>