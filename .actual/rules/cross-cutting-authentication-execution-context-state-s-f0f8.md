---
version: 2
name: cross-cutting-authentication-execution-context-state-s-f0f8
description: "Adoption of @openstatus/subscriptions Workspace Module for Service Notifications: Authentication Execution Context State Supplied Subscriber"
metadata:
  actual:
    adr_id: f0f8568d-6801-4fc0-93ad-4ac030cac19d
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

# Adoption of @openstatus/subscriptions Workspace Module for Service Notifications: Authentication Execution Context State Supplied Subscriber

Scope: applies to `packages/services/src/maintenance/**`, `packages/services/src/page-subscriber/**`, `packages/services/src/status-report/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-SUB-001** MUST: Authentication and execution context state MUST be supplied to subscriber notification routines through the shared service context and authentication modules.
- **R-SUB-002** MUST: Incoming service request arguments MUST undergo structured input validation parsing prior to invoking notification delivery logic.

### Verify

```bash
# Discover and execute the project static type validation script
# Discover and execute the workspace test suite covering service notification routines
```

**Accept when:**
- Static analysis and type checking pass across all service notification handlers with zero errors.
- Service test suites verify that notification endpoints successfully parse input schemas and delegate to @openstatus/subscriptions routines.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>