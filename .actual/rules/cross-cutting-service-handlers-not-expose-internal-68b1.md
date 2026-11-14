---
version: 2
name: cross-cutting-service-handlers-not-expose-internal-68b1
description: "Adoption of @openstatus/proto/status_page/v1 for Status Page RPC Interface Contracts: Service Handlers Not Expose Internal Database"
metadata:
  actual:
    adr_id: 68b1fa38-8c02-4ebf-b4ef-deae6ecc53ef
    status: accepted
    candidate_group_id: 5905ec44-eb02-4bd6-b2b2-575b7f1fb0f4
    evidence:
      source: deep
  scope:
    root: apps/server
    paths:
      - src/routes/rpc/handlers/status-page/**
---

# Adoption of @openstatus/proto/status_page/v1 for Status Page RPC Interface Contracts: Service Handlers Not Expose Internal Database

Scope: applies to `apps/server/src/routes/rpc/handlers/status-page/**`.

These rules are ALWAYS ACTIVE for all remote procedure call handlers responsible for status page domain operations and data conversion modules transforming internal persistence entities to and from status page transport contracts.

### Rules

- **R-STATUS-001** MUST_NOT: Service handlers MUST_NOT expose internal database schema entities directly over the remote procedure call boundary without explicit transformation to contract definitions from @openstatus/proto/status_page/v1.

### Verify

```bash
# Discover the repository test runner from the dependency manifest and execute the test suite covering status page remote procedure call handlers and converters.
# Discover the project static type checker and run type analysis to ensure strict adherence to protocol contract interfaces.
# Discover the repository linting and formatting scripts and execute them across the codebase to verify architectural boundary conformance.
```

**Accept when:**
- All status page remote procedure call handlers and data converters compile without type errors against the resolved contract package definitions.
- All automated test suites for status page endpoint handlers and data conversion modules pass successfully.
- No remote procedure call handler exposes database schema models directly across public response boundaries.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>