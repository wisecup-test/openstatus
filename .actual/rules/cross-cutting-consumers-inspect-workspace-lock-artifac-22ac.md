---
version: 2
name: cross-cutting-consumers-inspect-workspace-lock-artifac-22ac
description: "Adoption of @openstatus/notification-base for Notification Handlers: Consumers Inspect Workspace Lock Artifact Identify"
metadata:
  actual:
    adr_id: 22ac0ab4-3d77-4a68-8f7e-1e0a5c32dad1
    status: accepted
    candidate_group_id: bc30db3b-9d27-481a-aa30-7758c2739e46
    evidence:
      source: deep
  scope:
    root: packages/notifications/email
    paths:
      - src/**
---

# Adoption of @openstatus/notification-base for Notification Handlers: Consumers Inspect Workspace Lock Artifact Identify

Scope: applies to `packages/notifications/email/src/**`.

These rules are ALWAYS ACTIVE for all notification handler packages and transport dispatchers within the workspace defining incoming notification event consumption and dispatch workflows.

### Rules

- **R-ONB-001** MUST: Consumers MUST inspect the workspace lock artifact to identify the exact resolved version of internal and external dependencies before implementing or updating notification handlers.

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