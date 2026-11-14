---
version: 2
name: cross-cutting-notification-handlers-validate-incoming-a803
description: "Adoption of @openstatus/notification-base for Notification Handlers: Notification Handlers Validate Incoming Payload Data"
metadata:
  actual:
    adr_id: a8036c07-380f-4b2c-8afd-1912ada31cff
    status: accepted
    candidate_group_id: bc30db3b-9d27-481a-aa30-7758c2739e46
    evidence:
      source: deep
  scope:
    root: packages/notifications/email
    paths:
      - src/**
---

# Adoption of @openstatus/notification-base for Notification Handlers: Notification Handlers Validate Incoming Payload Data

Scope: applies to `packages/notifications/email/src/**`.

These rules are ALWAYS ACTIVE for all notification handler packages and transport dispatchers within the workspace.

### Rules

- **R-NOTIF-001** MUST: Notification handlers MUST validate incoming notification payload data using schema safeParse methods prior to executing delivery logic.

### Verify

```bash
# Discover and run the project's dependency analysis script to verify notification handlers reference the shared foundation module
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