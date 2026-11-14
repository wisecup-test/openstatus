---
version: 2
name: cross-cutting-notification-handlers-incorporate-domain-3fc4
description: "Adoption of @openstatus/notification-base for Notification Integrations: Notification Handlers Incorporate Domain Database Schema"
metadata:
  actual:
    adr_id: 3fc4129e-58ed-407d-b3d1-cffec7f6c5a0
    status: accepted
    candidate_group_id: 3f40d750-09fd-4476-b815-be9cf4a85d79
    evidence:
      source: deep
  scope:
    root: packages/notifications/bird-whatsapp
    paths:
      - src/**
---

# Adoption of @openstatus/notification-base for Notification Integrations: Notification Handlers Incorporate Domain Database Schema

Scope: applies to `packages/notifications/bird-whatsapp/src/**`.

These rules are ALWAYS ACTIVE for notification integration packages, provider delivery modules implementing outbound alerting or messaging, and components handling incoming notification dispatch events requiring payload parsing and external service delivery.

### Rules

- **R-NOTIF-001** MAY: Notification handlers MAY incorporate domain database schema models to map standardized dispatch records to external provider message payloads.

### Verify

```bash
# Discover and run the repository build validation script to ensure type-checking and interface conformance with the shared base package.
# Discover and execute the project test runner to validate notification dispatch handling and payload transformation behavior.
# Discover and execute the workspace linter to confirm adherence to package boundary and import restrictions.
```

**Accept when:**
- All notification provider modules successfully compile and satisfy interface contracts defined in @openstatus/notification-base.
- External dispatch execution tests pass with valid payload assertions and schema validation checks.
- Dependency boundaries and package resolution checks succeed without unresolved imports.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>