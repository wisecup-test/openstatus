---
version: 2
name: cross-cutting-consumers-inspect-workspace-dependency-m-130b
description: "Adoption of @openstatus/notification-base for Notification Dispatch Modules: Consumers Inspect Workspace Dependency Manifests Resolution"
metadata:
  actual:
    adr_id: 130b0750-cae7-4d89-987e-b7e2270681e4
    status: accepted
    candidate_group_id: 2cc45944-5dce-4b8e-9587-6ebcd38a2363
    evidence:
      source: deep
  scope:
    root: packages/notifications/pagerduty
    paths:
      - src/**
---

# Adoption of @openstatus/notification-base for Notification Dispatch Modules: Consumers Inspect Workspace Dependency Manifests Resolution

Scope: applies to `packages/notifications/pagerduty/src/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-NOTIF-001** MUST: Consumers MUST inspect the workspace dependency manifests and resolution artifacts to resolve and lock the exact version of internal and external dependencies prior to implementation.

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