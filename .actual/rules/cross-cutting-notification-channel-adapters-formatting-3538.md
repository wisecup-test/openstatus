---
version: 2
name: cross-cutting-notification-channel-adapters-formatting-3538
description: "Adoption of @openstatus/notification-base for Notification Packages: Notification Channel Adapters Formatting Modules Adopt"
metadata:
  actual:
    adr_id: 35388c27-f458-49da-a7f4-a926a1aef5c9
    status: accepted
    candidate_group_id: 76fa8e0d-c217-4490-ba72-6b02bcf6281e
    evidence:
      source: deep
  scope:
    root: packages/notifications/ms-teams
    paths:
      - src/**
---

# Adoption of @openstatus/notification-base for Notification Packages: Notification Channel Adapters Formatting Modules Adopt

Scope: applies to `packages/notifications/ms-teams/src/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-NOTIF-001** MUST: Notification channel adapters and formatting modules MUST adopt @openstatus/notification-base as their shared foundational dependency for notification context and contracts.

### Verify

```bash
# Discover and execute the repository dependency boundary validation check to verify that notification channel modules consume @openstatus/notification-base.
# Discover and execute the repository build and type verification suite to ensure contract alignment with @openstatus/notification-base.
```

**Accept when:**
- All notification integration modules consume shared notification types, interfaces, or context structures exclusively through @openstatus/notification-base.
- Static analysis and build checks confirm that notification channel adapters do not duplicate baseline notification contracts.

<enforcement>
Claude Code MUST NOT skip or defer verification. Verified by automated dependency analysis and peer review during continuous integration. Violation handling requires blocking PRs introducing ad-hoc notification structures and refactoring to adhere to shared notification contracts prior to merge.
</enforcement>