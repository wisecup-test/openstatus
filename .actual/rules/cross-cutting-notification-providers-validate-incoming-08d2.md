---
version: 2
name: cross-cutting-notification-providers-validate-incoming-08d2
description: "Adoption of @openstatus/notification-base for Notification Integrations: Notification Providers Validate Incoming Serialized Payloads"
metadata:
  actual:
    adr_id: 08d240ae-9335-4ff2-a034-2566f8fbaedc
    status: accepted
    candidate_group_id: 3f40d750-09fd-4476-b815-be9cf4a85d79
    evidence:
      source: deep
  scope:
    root: packages/notifications/bird-whatsapp
    paths:
      - src/**
---

# Adoption of @openstatus/notification-base for Notification Integrations: Notification Providers Validate Incoming Serialized Payloads

Scope: applies to `packages/notifications/bird-whatsapp/src/**`.

These rules are ALWAYS ACTIVE for notification integration packages and provider delivery modules implementing outbound alerting or messaging.

### Rules

- **R-NOTIF-001** MUST: Notification providers MUST validate incoming serialized notification payloads against designated structural schemas before executing external provider client dispatches.

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
Claude Code MUST NOT skip or defer verification. Verification is mandatory via automated CI and peer review.
</enforcement>