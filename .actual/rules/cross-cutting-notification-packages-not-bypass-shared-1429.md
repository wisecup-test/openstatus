---
version: 2
name: cross-cutting-notification-packages-not-bypass-shared-1429
description: "Adoption of @openstatus/notification-base for Notification Handlers: Notification Packages Not Bypass Shared Foundation"
metadata:
  actual:
    adr_id: 1429e993-0500-4bd6-8d08-736ca3274480
    status: accepted
    candidate_group_id: bc30db3b-9d27-481a-aa30-7758c2739e46
    evidence:
      source: deep
  scope:
    root: packages/notifications/email
    paths:
      - src/**
---

# Adoption of @openstatus/notification-base for Notification Handlers: Notification Packages Not Bypass Shared Foundation

Scope: applies to `packages/notifications/email/src/**`.

These rules are ALWAYS ACTIVE for all notification handler packages and transport dispatchers within the workspace when defining incoming notification event consumption and dispatch workflows.

### Rules

- **R-NOTIF-001** MUST_NOT: Notification packages MUST NOT bypass the shared notification foundation module to implement isolated notification payload parsing.

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
Claude Code MUST NOT skip or defer verification. All pull requests violating architectural constraints or bypassing the foundation module will be blocked until rectified.
</enforcement>