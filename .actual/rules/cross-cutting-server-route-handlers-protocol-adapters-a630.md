---
version: 2
name: cross-cutting-server-route-handlers-protocol-adapters-a630
description: "Adoption of @openstatus/services as the Shared Domain Service Module: Server Route Handlers Protocol Adapters Delegate"
metadata:
  actual:
    adr_id: a630bd04-3615-4304-9d74-99636af0a7cd
    status: accepted
    candidate_group_id: 59a6b1ea-bddf-485b-a89f-dea1337ef130
    evidence:
      source: deep
  scope:
    root: apps/server
    paths:
      - src/routes/slack/**
---

# Adoption of @openstatus/services as the Shared Domain Service Module: Server Route Handlers Protocol Adapters Delegate

Scope: applies to `apps/server/src/routes/slack/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-SRV-001** MUST: All server route handlers and protocol adapters MUST delegate domain business logic and shared operations to @openstatus/services rather than executing domain logic directly within transport-layer handlers.

### Verify

```bash
# Discover the root project manifest and execute the designated workspace test suite covering route integration and service adapter modules.
# Discover the repository linter and type-checking scripts from the workspace configuration to validate import boundary compliance across route handlers.
```

**Accept when:**
- All server route handlers and protocol adapters route domain operations through @openstatus/services rather than executing domain logic inline.
- Project verification scripts and static analysis suites complete with zero boundary violations.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>