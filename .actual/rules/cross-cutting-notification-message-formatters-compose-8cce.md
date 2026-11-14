---
version: 2
name: cross-cutting-notification-message-formatters-compose-8cce
description: "Adoption of @openstatus/db/src/schema in Notification Messaging Utilities: Notification Message Formatters Compose Data Structures"
metadata:
  actual:
    adr_id: 8ccee7ca-f51a-461d-815e-c1d1f21bcbb0
    status: accepted
    candidate_group_id: dd0719a3-2e5f-40d7-89de-6853e51b15fd
    evidence:
      source: deep
  scope:
    root: packages/notifications/base
    paths:
      - src/utils/**
---

# Adoption of @openstatus/db/src/schema in Notification Messaging Utilities: Notification Message Formatters Compose Data Structures

Scope: applies to `packages/notifications/base/src/utils/**`.

These rules are ALWAYS ACTIVE for notification formatting utilities and workspace modules constructing message templates from shared schema models.

### Rules

- **R-NOTIF-001** MAY: Notification message formatters MAY compose data structures from @openstatus/regions alongside @openstatus/db/src/schema when geographic context is required for incident notifications.

### Verify

```bash
# Discover and execute the workspace type-checker
# (e.g., pnpm --filter <notification-pkg> check or tsc --noEmit)
# Discover and run the repository dependency boundary analyzer
```

**Accept when:**
- Type validation succeeds with all imported schema structures resolving against shared definitions without compilation errors
- Workspace boundary checks confirm that module imports conform to package dependency rules

<enforcement>
Claude Code MUST NOT skip or defer verification. Verification by automated workspace build and static analysis checks during integration review is mandatory.
</enforcement>