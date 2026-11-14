---
version: 2
name: cross-cutting-notification-provider-modules-not-bypass-027d
description: "Adoption of @openstatus/notification-base for Notification Dispatch Modules: Notification Provider Modules Not Bypass Shared"
metadata:
  actual:
    adr_id: 027d1a08-6d46-4336-9fac-ba5609b00c22
    status: accepted
    candidate_group_id: 2cc45944-5dce-4b8e-9587-6ebcd38a2363
    evidence:
      source: deep
  scope:
    root: packages/notifications/pagerduty
    paths:
      - src/**
---

# Adoption of @openstatus/notification-base for Notification Dispatch Modules: Notification Provider Modules Not Bypass Shared

Scope: applies to `packages/notifications/pagerduty/src/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-NOTIF-001** MUST_NOT: Notification provider modules MUST NOT bypass the shared notification base contracts or construct ad-hoc dispatch pipelines that decouple from core schema definitions.

### Verify

```bash
# Discover and execute the project workspace test suite covering notification packages.
# Discover and execute the repository type-checking and schema validation verification scripts across notification modules.
```

**Accept when:**
- All notification packages pass workspace verification scripts without schema parsing or type errors.
- Notification payload parsing and outbound event payload schemas validate successfully during automated test execution.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>