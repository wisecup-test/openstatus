---
version: 2
name: cross-cutting-service-layer-notification-handlers-subs-e8fe
description: "Adoption of @openstatus/subscriptions Workspace Module for Service Notifications: Service Layer Notification Handlers Subscriber Delivery"
metadata:
  actual:
    adr_id: e8fe393b-f564-432b-8672-94a37ae34bd1
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

# Adoption of @openstatus/subscriptions Workspace Module for Service Notifications: Service Layer Notification Handlers Subscriber Delivery

Scope: applies to `packages/services/src/maintenance/**`, `packages/services/src/page-subscriber/**`, `packages/services/src/status-report/**`.

These rules are ALWAYS ACTIVE for service-layer handlers, endpoints, and notification workflows that broadcast notifications, execute test webhooks, or process subscriber communication events.

### Rules

- **R-SUB-001** MUST: All service-layer notification handlers and subscriber delivery routines MUST use the @openstatus/subscriptions workspace module as the authoritative domain library for subscriber resolution and notification delivery.

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