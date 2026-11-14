---
version: 2
name: cross-cutting-endpoint-handlers-isolate-internal-persi-c571
description: "Adoption of @openstatus/proto/status_page/v1 for Status Page RPC Interface Contracts: Endpoint Handlers Isolate Internal Persistence Schemas"
metadata:
  actual:
    adr_id: c57139ca-fa9b-4667-b214-550ca1e9a5eb
    status: accepted
    candidate_group_id: 5905ec44-eb02-4bd6-b2b2-575b7f1fb0f4
    evidence:
      source: deep
  scope:
    root: apps/server
    paths:
      - src/routes/rpc/handlers/status-page/**
---

# Adoption of @openstatus/proto/status_page/v1 for Status Page RPC Interface Contracts: Endpoint Handlers Isolate Internal Persistence Schemas

Scope: applies to `apps/server/src/routes/rpc/handlers/status-page/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-SP-001** MUST: Endpoint handlers MUST isolate internal persistence schemas from wire transport types through dedicated converter functions.

### Verify

```bash
# Discover and run the project test suite covering status page remote procedure call handlers and converters
# Discover the project static type checker and run type analysis
# Discover the repository linting and formatting scripts and execute them
```

**Accept when:**
- All status page remote procedure call handlers and data converters compile without type errors against the resolved contract package definitions.
- All automated test suites for status page endpoint handlers and data conversion modules pass successfully.
- No remote procedure call handler exposes database schema models directly across public response boundaries.

<enforcement>
Claude Code MUST NOT skip or defer verification. Verification is mandatory via automated continuous integration pipelines, peer code reviews, and static analysis checking for unauthorized imports or direct database schema exposure across route handlers.
</enforcement>