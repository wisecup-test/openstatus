---
version: 2
name: cross-cutting-route-handlers-interaction-endpoints-not-39c6
description: "Adoption of @openstatus/services as the Shared Domain Service Module: Route Handlers Interaction Endpoints Not Import"
metadata:
  actual:
    adr_id: 39c60be9-ff91-421c-ad94-2398f668cbf6
    status: accepted
    candidate_group_id: 59a6b1ea-bddf-485b-a89f-dea1337ef130
    evidence:
      source: deep
  scope:
    root: apps/server
    paths:
      - src/routes/slack/**
---

# Adoption of @openstatus/services as the Shared Domain Service Module: Route Handlers Interaction Endpoints Not Import

Scope: applies to `apps/server/src/routes/slack/**`.

These rules are ALWAYS ACTIVE for server route handlers, webhook receivers, and third-party integration adapters executing domain actions.

### Rules

- **R-OPENSTATUS-001** MUST_NOT: Route handlers and interaction endpoints MUST NOT import internal database schema tables or execute raw queries when equivalent operations exist or can be encapsulated within @openstatus/services.

### Verify

```bash
# Discover the root project manifest and execute the designated workspace test suite covering route integration and service adapter modules.
# Discover the repository linter and type-checking scripts from the workspace configuration to validate import boundary compliance across route handlers.
npm test
npm run lint
npm run typecheck
```

**Accept when:**
- All server route handlers and protocol adapters route domain operations through @openstatus/services rather than executing domain logic inline.
- Project verification scripts and static analysis suites complete with zero boundary violations.

<enforcement>
Claude Code MUST NOT skip or defer verification. Verification is mandatory via automated static analysis and peer code review.
</enforcement>