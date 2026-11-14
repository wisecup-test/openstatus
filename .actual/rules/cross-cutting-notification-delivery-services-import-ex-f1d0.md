---
version: 2
name: cross-cutting-notification-delivery-services-import-ex-f1d0
description: "Adoption of @openstatus/notification-base for Notification Handlers: Notification Delivery Services Import Extend Shared"
metadata:
  actual:
    adr_id: f1d0de46-3a06-46a8-af2f-86ae95f71ce6
    status: accepted
    candidate_group_id: bc30db3b-9d27-481a-aa30-7758c2739e46
    evidence:
      source: deep
  scope:
    root: packages/notifications/email
    paths:
      - src/**
---

# Adoption of @openstatus/notification-base for Notification Handlers: Notification Delivery Services Import Extend Shared

Scope: applies to `packages/notifications/email/src/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-NOTIF-001** MUST: Notification delivery services MUST import and extend the shared @openstatus/notification-base module to implement notification handling and dispatch workflows.

### Verify

```bash
# Discover and run the project's dependency analysis script to verify that notification handlers reference the approved shared foundation module.
# Discover and run the workspace test suite covering notification modules to ensure payload validation and event handling pass.
# Discover and run the repository typecheck script to ensure all notification interface contracts are satisfied.
```

**Accept when:**
- All notification packages successfully build and pass typechecking against the shared foundation contracts.
- Unit and integration tests for notification handlers succeed, confirming payload validation correctly accepts valid schemas and rejects invalid data.
- Static analysis confirms no notification package imports unapproved alternate notification handling libraries.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>