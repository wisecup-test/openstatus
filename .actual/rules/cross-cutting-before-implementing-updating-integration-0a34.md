---
version: 2
name: cross-cutting-before-implementing-updating-integration-0a34
description: "Adoption of @openstatus/notification-base for Notification Provider Integration: Before Implementing Updating Integrations Openstatus Notification"
metadata:
  actual:
    adr_id: 0a347e42-e82f-496e-b90f-fe9b7acb79bb
    status: accepted
    candidate_group_id: f38733e0-f502-4a66-81d9-7b70c2173a8d
    evidence:
      source: deep
  scope:
    root: packages/notifications/telegram
    paths:
      - src/**
---

# Adoption of @openstatus/notification-base for Notification Provider Integration: Before Implementing Updating Integrations Openstatus Notification

Scope: applies to `packages/notifications/telegram/src/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-NOTIF-001** MUST: Before implementing or updating integrations using @openstatus/notification-base, consumers MUST discover the project dependency manifest and lock artifact to resolve the exact dependency versions and interface definitions.

### Verify

```bash
# Discover and run the project's workspace-wide static type-checking script to confirm full contract adherence with @openstatus/notification-base.
# Discover and execute the test suite corresponding to notification delivery integrations to verify payload parsing and external dispatch behaviors.
```

**Accept when:**
- All notification provider packages compile without type errors against the imported base contract.
- Unit and integration tests for notification dispatch pass successfully, confirming valid schema parsing and external request dispatching.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>