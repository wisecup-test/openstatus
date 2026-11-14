---
version: 2
name: cross-cutting-notification-provider-packages-not-const-80b4
description: "Adoption of @openstatus/notification-base for Notification Provider Integration: Notification Provider Packages Not Construct Hoc"
metadata:
  actual:
    adr_id: 80b478c6-a856-47cf-a347-4b62d766c78b
    status: accepted
    candidate_group_id: f38733e0-f502-4a66-81d9-7b70c2173a8d
    evidence:
      source: deep
  scope:
    root: packages/notifications/telegram
    paths:
      - src/**
---

# Adoption of @openstatus/notification-base for Notification Provider Integration: Notification Provider Packages Not Construct Hoc

Scope: applies to `packages/notifications/telegram/src/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-NOTIF-001** MUST_NOT: Notification provider packages MUST NOT construct ad-hoc internal notification schema definitions that bypass the shared foundational types established by @openstatus/notification-base.

### Verify

```bash
# Discover and run the project's workspace-wide static type-checking script to confirm full contract adherence with @openstatus/notification-base.
# Discover and execute the test suite corresponding to notification delivery integrations to verify payload parsing and external dispatch behaviors.
```

**Accept when:**
- All notification provider packages compile without type errors against the imported base contract.
- Unit and integration tests for notification dispatch pass successfully, confirming valid schema parsing and external request dispatching.

<enforcement>
Claude Code MUST NOT skip or defer verification. Continuous integration type checking and static analysis workflows enforce verification across all workspace packages.
</enforcement>