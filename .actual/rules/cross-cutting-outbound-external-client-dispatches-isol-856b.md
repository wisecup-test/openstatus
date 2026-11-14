---
version: 2
name: cross-cutting-outbound-external-client-dispatches-isol-856b
description: "Adoption of @openstatus/notification-base for Notification Integrations: Outbound External Client Dispatches Isolate Provider"
metadata:
  actual:
    adr_id: 856b55c7-733f-4507-8ff3-83662025e26b
    status: accepted
    candidate_group_id: 3f40d750-09fd-4476-b815-be9cf4a85d79
    evidence:
      source: deep
  scope:
    root: packages/notifications/bird-whatsapp
    paths:
      - src/**
---

# Adoption of @openstatus/notification-base for Notification Integrations: Outbound External Client Dispatches Isolate Provider

Scope: applies to `packages/notifications/bird-whatsapp/src/**`.

These rules are ALWAYS ACTIVE for notification integration packages, provider delivery modules implementing outbound alerting or messaging, and components handling incoming notification dispatch events requiring payload parsing and external service delivery.

### Rules

- **R-NOTIF-001** SHOULD: Outbound external client dispatches SHOULD isolate provider credentials and endpoint configurations within validated environment bindings rather than hardcoding connection values.

### Verify

```bash
# Discover and run the repository build validation script
# Discover and execute the project test runner
# Discover and execute the workspace linter
```

**Accept when:**
- All notification provider modules successfully compile and satisfy interface contracts defined in @openstatus/notification-base.
- External dispatch execution tests pass with valid payload assertions and schema validation checks.
- Dependency boundaries and package resolution checks succeed without unresolved imports.

<enforcement>
Claude Code MUST NOT skip or defer verification. All notification provider modules must satisfy interface contracts, pass dispatch execution tests, and maintain valid dependency boundaries.
</enforcement>