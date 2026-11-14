---
version: 2
name: cross-cutting-notification-channel-packages-not-define-88b8
description: "Adoption of @openstatus/notification-base for Notification Channel Modules: Notification Channel Packages Not Define Duplicate"
metadata:
  actual:
    adr_id: 88b89b0d-4331-46e1-ad5b-4767f40f66e6
    status: accepted
    candidate_group_id: d4bf6f6d-dbc1-4ba6-ab0c-83430fda99ff
    evidence:
      source: deep
  scope:
    root: packages/notifications/slack
    paths:
      - src/**
---

# Adoption of @openstatus/notification-base for Notification Channel Modules: Notification Channel Packages Not Define Duplicate

Scope: applies to `packages/notifications/slack/src/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-NOTIF-001** MUST_NOT: Notification channel packages MUST NOT define duplicate notification domain primitives or bypass @openstatus/notification-base to establish uncoordinated payload contracts.

### Verify

```bash
# Discover the workspace dependency graph and verify that notification channel packages declare and resolve a dependency on @openstatus/notification-base.
# Discover and execute the repository static analysis and typecheck verification routines to validate that notification channel modules compile cleanly against @openstatus/notification-base.
```

**Accept when:**
- Static analysis and type checking complete with zero errors across all notification channel packages importing @openstatus/notification-base.
- Dependency graph inspection confirms all notification modules resolve @openstatus/notification-base consistently according to the repository resolution artifact.

<enforcement>
Claude Code MUST NOT skip or defer verification. Automated workspace dependency boundary checks, static analysis, and mandatory peer code review enforce these rules.
</enforcement>