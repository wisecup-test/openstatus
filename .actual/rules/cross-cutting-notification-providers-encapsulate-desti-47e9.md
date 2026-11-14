---
version: 2
name: cross-cutting-notification-providers-encapsulate-desti-47e9
description: "Adoption of @openstatus/notification-base for Notification Dispatch Modules: Notification Providers Encapsulate Destination Specific Payload"
metadata:
  actual:
    adr_id: 47e91675-7dc5-4295-9024-c5e102c2d24f
    status: accepted
    candidate_group_id: 2cc45944-5dce-4b8e-9587-6ebcd38a2363
    evidence:
      source: deep
  scope:
    root: packages/notifications/pagerduty
    paths:
      - src/**
---

# Adoption of @openstatus/notification-base for Notification Dispatch Modules: Notification Providers Encapsulate Destination Specific Payload

Scope: applies to `packages/notifications/pagerduty/src/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-NOTIF-001** SHOULD: Notification providers SHOULD encapsulate destination-specific payload schema definitions within local provider configuration schemas and verify event payloads prior to network transmission.

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