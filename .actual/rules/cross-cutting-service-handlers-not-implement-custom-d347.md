---
version: 2
name: cross-cutting-service-handlers-not-implement-custom-d347
description: "Adoption of @openstatus/subscriptions Workspace Module for Service Notifications: Service Handlers Not Implement Custom Hoc"
metadata:
  actual:
    adr_id: d347fdc0-1aea-4a95-950e-daa84d73ddfe
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

# Adoption of @openstatus/subscriptions Workspace Module for Service Notifications: Service Handlers Not Implement Custom Hoc

Scope: applies to `packages/services/src/maintenance/**`, `packages/services/src/page-subscriber/**`, `packages/services/src/status-report/**`.

These rules are ALWAYS ACTIVE for service-layer handlers, endpoints, and workflows that broadcast notifications, execute test webhooks, or process subscriber communication events.

### Rules

- **R-SUB-001** MUST_NOT: Service handlers MUST NOT implement custom or ad-hoc subscriber dispatch logic, direct third-party webhook delivery pipelines, or channel notification loops outside of @openstatus/subscriptions.
- **R-SUB-002** MUST: All service notification handlers parse and sanitize input arguments through their declared schema validation routines before invoking @openstatus/subscriptions functions.
- **R-SUB-003** MUST: Pass shared context and authentication objects into subscription handlers to preserve tenancy, audit trails, and execution context throughout notification dispatch.

### Verify

```bash
# Discover and execute the project static type validation script
# (e.g., pnpm check-types or tsc --noEmit)

# Discover and execute the workspace test suite covering service notification routines
# (e.g., pnpm test)
```

**Accept when:**
- Static analysis and type checking pass across all service notification handlers with zero errors.
- Service test suites verify that notification endpoints successfully parse input schemas and delegate to @openstatus/subscriptions routines.

<enforcement>
Claude Code MUST NOT skip or defer verification. Continuous integration automated static type verification and test suite execution are mandatory.
</enforcement>