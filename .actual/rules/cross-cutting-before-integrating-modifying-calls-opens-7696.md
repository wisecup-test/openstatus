---
version: 2
name: cross-cutting-before-integrating-modifying-calls-opens-7696
description: "Adoption of @openstatus/subscriptions Workspace Module for Service Notifications: Before Integrating Modifying Calls Openstatus Subscriptions"
metadata:
  actual:
    adr_id: 7696b7e0-85cf-4834-855b-2f530fb4ff88
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

# Adoption of @openstatus/subscriptions Workspace Module for Service Notifications: Before Integrating Modifying Calls Openstatus Subscriptions

Scope: applies to `packages/services/src/maintenance/**`, `packages/services/src/page-subscriber/**`, `packages/services/src/status-report/**`.

These rules are ALWAYS ACTIVE for service-layer handlers, endpoints, and notification workflows that broadcast notifications, execute test webhooks, or process subscriber communication events.

### Rules

- **R-OPEN-001** MUST: Before integrating or modifying calls to @openstatus/subscriptions, the consumer MUST discover the workspace dependency manifest and inspect the repository resolution lock artifact to establish the exact resolved workspace package version.

### Verify

```bash
# Discover and execute the project static type validation script to verify that all service notification entrypoints import and consume @openstatus/subscriptions without type errors.
# Discover and execute the workspace test suite covering service notification routines to confirm successful subscriber resolution and webhook dispatch behavior.
```

**Accept when:**
- Static analysis and type checking pass across all service notification handlers with zero errors.
- Service test suites verify that notification endpoints successfully parse input schemas and delegate to @openstatus/subscriptions routines.

<enforcement>
Claude Code MUST NOT skip or defer verification. Verified by continuous integration automated static type verification, test suite execution, and mandatory peer code review.
</enforcement>