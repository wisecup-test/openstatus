---
version: 2
name: cross-cutting-integration-adapters-maintain-local-ephe-9759
description: "Adoption of @openstatus/services as the Shared Domain Service Module: Integration Adapters Maintain Local Ephemeral Confirmation"
metadata:
  actual:
    adr_id: 97595e90-8c66-4690-8fbe-bb3ecc80be36
    status: accepted
    candidate_group_id: 59a6b1ea-bddf-485b-a89f-dea1337ef130
    evidence:
      source: deep
  scope:
    root: apps/server
    paths:
      - src/routes/slack/**
---

# Adoption of @openstatus/services as the Shared Domain Service Module: Integration Adapters Maintain Local Ephemeral Confirmation

Scope: applies to `apps/server/src/routes/slack/**`.

These rules are ALWAYS ACTIVE for server route handlers, webhook receivers, and third-party integration adapters executing domain actions.

### Rules

- **R-DOM-001** MAY: Integration adapters MAY maintain local ephemeral confirmation stores and message block formatting helpers outside @openstatus/services when handling transport-specific interactive payloads.

### Verify

```bash
# Discover the root project manifest and execute the designated workspace test suite covering route integration and service adapter modules.
# Discover the repository linter and type-checking scripts from the workspace configuration to validate import boundary compliance across route handlers.
npx turbo run test lint typecheck
```

**Accept when:**
- All server route handlers and protocol adapters route domain operations through @openstatus/services rather than executing domain logic inline.
- Project verification scripts and static analysis suites complete with zero boundary violations.

<enforcement>
Claude Code MUST NOT skip or defer verification. All new route handlers and integration adapters are verified by automated static analysis verifying import boundaries and peer code review.
</enforcement>