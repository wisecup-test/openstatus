---
version: 2
name: cross-cutting-shared-database-entity-interactions-requ-7b81
description: "Adoption of @openstatus/subscriptions Workspace Module for Service Notifications: Shared Database Entity Interactions Required During"
metadata:
  actual:
    adr_id: 7b81cd6e-5a61-4473-9bd3-49170889d2a6
    status: accepted
    candidate_group_id: 661dd269-6c9f-4493-9f3c-300f559dd7a2
    evidence:
      source: deep
  scope:
    root: packages/services
    paths:
      - src/maintenance/**
      - src/page-subscriber/**
      - src/status-report/**
---

# Adoption of @openstatus/subscriptions Workspace Module for Service Notifications: Shared Database Entity Interactions Required During

Scope: applies to `packages/services/src/maintenance/**`, `packages/services/src/page-subscriber/**`, `packages/services/src/status-report/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-SUB-001** SHOULD: Shared database entity interactions required during notification processing SHOULD coordinate with the centralized database and schema packages alongside @openstatus/subscriptions.

### Verify

```bash
# Discover and execute the project static type validation script to verify that all service notification entrypoints import and consume @openstatus/subscriptions without type errors.
# Discover and execute the workspace test suite covering service notification routines to confirm successful subscriber resolution and webhook dispatch behavior.
```

**Accept when:**
- Static analysis and type checking pass across all service notification handlers with zero errors.
- Service test suites verify that notification endpoints successfully parse input schemas and delegate to @openstatus/subscriptions routines.

<enforcement>
Claude Code MUST NOT skip or defer verification. Continuous integration automated static type verification, test suite execution, and mandatory peer code review are required.
</enforcement>