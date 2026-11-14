---
version: 2
name: cross-cutting-handlers-perform-safe-validation-dynamic-c69a
description: "Adoption of @openstatus/proto/status_page/v1 for Status Page RPC Interface Contracts: Handlers Perform Safe Validation Dynamic Configuration"
metadata:
  actual:
    adr_id: c69af8e7-c2fb-4a92-a5bc-22a8b270119e
    status: accepted
    candidate_group_id: 5905ec44-eb02-4bd6-b2b2-575b7f1fb0f4
    evidence:
      source: deep
  scope:
    root: apps/server
    paths:
      - src/routes/rpc/handlers/status-page/**
---

# Adoption of @openstatus/proto/status_page/v1 for Status Page RPC Interface Contracts: Handlers Perform Safe Validation Dynamic Configuration

Scope: applies to `apps/server/src/routes/rpc/handlers/status-page/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-STATUS-001** SHOULD: Handlers SHOULD perform safe validation on dynamic configuration payloads before mapping them to protocol contract representations.

### Verify

```bash
# Discover and run test suite for status page handlers and converters
# Discover and run static type checking
# Discover and run repository linting and formatting scripts
```

**Accept when:**
- All status page remote procedure call handlers and data converters compile without type errors against the resolved contract package definitions.
- All automated test suites for status page endpoint handlers and data conversion modules pass successfully.
- No remote procedure call handler exposes database schema models directly across public response boundaries.

<enforcement>
Claude Code MUST NOT skip or defer verification. All status page remote procedure call handlers and data converters must strictly adhere to protocol contract interfaces and pass all verification suites.
</enforcement>