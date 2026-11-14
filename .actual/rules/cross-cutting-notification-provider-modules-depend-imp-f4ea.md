---
version: 2
name: cross-cutting-notification-provider-modules-depend-imp-f4ea
description: "Adoption of @openstatus/notification-base for Notification Dispatch Modules: Notification Provider Modules Depend Implement Shared"
metadata:
  actual:
    adr_id: f4ea5962-8c41-4d3f-84b3-c1d082023725
    status: accepted
    candidate_group_id: 2cc45944-5dce-4b8e-9587-6ebcd38a2363
    evidence:
      source: deep
  scope:
    root: packages/notifications/pagerduty
    paths:
      - src/**
---

# Adoption of @openstatus/notification-base for Notification Dispatch Modules: Notification Provider Modules Depend Implement Shared

Scope: applies to `packages/notifications/pagerduty/src/**`.

These rules are ALWAYS ACTIVE for all notification provider modules and integration packages delivering alert events to external services, and modules that ingest alert data and dispatch outbound notifications across supported channels.

### Rules

- **R-NOTIF-001** MUST: Notification provider modules MUST depend on and implement the shared @openstatus/notification-base module abstractions to define and execute outbound alert notification handlers.

### Verify

```bash
# Discover and execute the project workspace test suite covering notification packages
# Discover and execute the repository type-checking and schema validation verification scripts across notification modules
```

**Accept when:**
- All notification packages pass workspace verification scripts without schema parsing or type errors.
- Notification payload parsing and outbound event payload schemas validate successfully during automated test execution.

<enforcement>
Claude Code MUST NOT skip or defer verification. Automated continuous integration test and lint validation pipelines verify compliance; pull requests bypassing the base notification library or introducing unvalidated payload handling will be rejected.
</enforcement>