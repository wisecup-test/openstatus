---
version: 2
name: cross-cutting-modules-constructing-notification-messag-4a25
description: "Adoption of @openstatus/db/src/schema in Notification Messaging Utilities: Modules Constructing Notification Messages Source Entity"
metadata:
  actual:
    adr_id: 4a25864b-6bcb-4124-8544-a72592a5a01a
    status: accepted
    candidate_group_id: dd0719a3-2e5f-40d7-89de-6853e51b15fd
    evidence:
      source: deep
  scope:
    root: packages/notifications/base
    paths:
      - src/utils/**
---

# Adoption of @openstatus/db/src/schema in Notification Messaging Utilities: Modules Constructing Notification Messages Source Entity

Scope: applies to `packages/notifications/base/src/utils/**`.

These rules are ALWAYS ACTIVE for notification formatting utilities requiring database entity structure representations and workspace modules constructing message templates from shared schema models.

### Rules

- **R-NOTIF-001** MUST: Modules constructing notification messages MUST source entity data models directly from @openstatus/db/src/schema rather than re-declaring redundant local entity structures.

### Verify

```bash
# Discover and execute the workspace type-checker to verify cross-module schema import compatibility
# Discover and run the repository dependency boundary analyzer to confirm valid workspace package references
```

**Accept when:**
- Type validation succeeds with all imported schema structures resolving against shared definitions without compilation errors
- Workspace boundary checks confirm that module imports conform to package dependency rules

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>