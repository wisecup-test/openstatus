---
version: 2
name: cross-cutting-notification-dispatch-handlers-handle-pa-1d49
description: "Adoption of @openstatus/notification-base for Notification Handlers: Notification Dispatch Handlers Handle Payload Validation"
metadata:
  actual:
    adr_id: 1d49418b-9e27-42ae-b205-5fa261bfb4e4
    status: accepted
    candidate_group_id: bc30db3b-9d27-481a-aa30-7758c2739e46
    evidence:
      source: deep
  scope:
    root: packages/notifications/email
    paths:
      - src/**
---

# Adoption of @openstatus/notification-base for Notification Handlers: Notification Dispatch Handlers Handle Payload Validation

Scope: applies to `packages/notifications/email/src/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-NOTIF-001** SHOULD: Notification dispatch handlers SHOULD handle payload validation failures gracefully by returning an error result without uncaught exceptions.

### Verify

```bash
# Discover and run the project's dependency analysis script to verify notification handlers reference approved shared foundation module
# Discover and run the workspace test suite covering notification modules
# Discover and run the repository typecheck script
```

**Accept when:**
- All notification packages successfully build and pass typechecking against the shared foundation contracts.
- Unit and integration tests for notification handlers succeed, confirming payload validation correctly accepts valid schemas and rejects invalid data.
- Static analysis confirms no notification package imports unapproved alternate notification handling libraries.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>