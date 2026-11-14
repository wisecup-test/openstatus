---
version: 2
name: cross-cutting-notification-utilities-not-mutate-direct-0b90
description: "Adoption of @openstatus/db/src/schema in Notification Messaging Utilities: Notification Utilities Not Mutate Directly Expose"
metadata:
  actual:
    adr_id: 0b90e348-5ae6-4d0c-9a82-2d3fe9c6ae58
    status: accepted
    candidate_group_id: dd0719a3-2e5f-40d7-89de-6853e51b15fd
    evidence:
      source: deep
  scope:
    root: packages/notifications/base
    paths:
      - src/utils/**
---

# Adoption of @openstatus/db/src/schema in Notification Messaging Utilities: Notification Utilities Not Mutate Directly Expose

Scope: applies to `packages/notifications/base/src/utils/**`.

These rules are ALWAYS ACTIVE for notification formatting utilities requiring database entity structure representations and workspace modules constructing message templates from shared schema models.

### Rules

- **R-NOTIF-001** SHOULD_NOT: Notification utilities SHOULD_NOT mutate or directly expose internal database columns from @openstatus/db/src/schema that are outside public notification payload requirements.

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