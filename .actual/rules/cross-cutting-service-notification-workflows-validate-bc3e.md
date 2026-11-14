---
version: 2
name: cross-cutting-service-notification-workflows-validate-bc3e
description: "Adoption of @openstatus/subscriptions Workspace Module for Service Notifications: Service Notification Workflows Validate Input Parameters"
metadata:
  actual:
    adr_id: bc3eb431-c5e8-4683-bd42-1aa1fefdc5b7
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

# Adoption of @openstatus/subscriptions Workspace Module for Service Notifications: Service Notification Workflows Validate Input Parameters

Scope: applies to `packages/services/src/maintenance/**`, `packages/services/src/page-subscriber/**`, `packages/services/src/status-report/**`.

These rules are ALWAYS ACTIVE for service-layer handlers and endpoints that broadcast notifications, execute test webhooks, or process subscriber communication events.

### Rules

- **R-NOTIF-001** MUST: Service notification workflows MUST validate input parameters using their designated schema parse routines prior to invoking @openstatus/subscriptions dispatch methods.

### Verify

```bash
# Discover and execute the project static type validation script
# Discover and execute the workspace test suite covering service notification routines
```

**Accept when:**
- Static analysis and type checking pass across all service notification handlers with zero errors.
- Service test suites verify that notification endpoints successfully parse input schemas and delegate to @openstatus/subscriptions routines.

<enforcement>
Claude Code MUST NOT skip or defer verification. Continuous integration automated static type verification and test suite execution are mandatory.
</enforcement>