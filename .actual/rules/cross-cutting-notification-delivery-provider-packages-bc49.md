---
version: 2
name: cross-cutting-notification-delivery-provider-packages-bc49
description: "Adoption of @openstatus/notification-base for Notification Integrations: Notification Delivery Provider Packages Consume Openstatus"
metadata:
  actual:
    adr_id: bc4931cb-8ff1-43a3-9079-3417b7271d54
    status: accepted
    candidate_group_id: 3f40d750-09fd-4476-b815-be9cf4a85d79
    evidence:
      source: deep
  scope:
    root: packages/notifications/bird-whatsapp
    paths:
      - src/**
---

# Adoption of @openstatus/notification-base for Notification Integrations: Notification Delivery Provider Packages Consume Openstatus

Scope: applies to `packages/notifications/bird-whatsapp/src/**`.

These rules are ALWAYS ACTIVE for all notification integration packages, provider delivery modules implementing outbound alerting or messaging, and components handling incoming notification dispatch events.

### Rules

- **R-NOTIF-001** MUST: Notification delivery provider packages MUST consume @openstatus/notification-base as the foundational module for notification integration interfaces, base contracts, and lifecycle coordination.

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
Claude Code MUST NOT skip or defer verification. Automated continuous integration pipeline running repository build and test discovery scripts, alongside peer architectural review, validates compliance. Violations will block PR merges.
</enforcement>