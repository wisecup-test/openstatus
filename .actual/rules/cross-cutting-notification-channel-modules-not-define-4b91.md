---
version: 2
name: cross-cutting-notification-channel-modules-not-define-4b91
description: "Adoption of @openstatus/notification-base for Notification Packages: Notification Channel Modules Not Define Duplicate"
metadata:
  actual:
    adr_id: 4b919c53-3e67-4ab3-9ad3-e2c5623ef8ca
    status: accepted
    candidate_group_id: 76fa8e0d-c217-4490-ba72-6b02bcf6281e
    evidence:
      source: deep
  scope:
    root: packages/notifications/ms-teams
    paths:
      - src/**
---

# Adoption of @openstatus/notification-base for Notification Packages: Notification Channel Modules Not Define Duplicate

Scope: applies to `packages/notifications/ms-teams/src/**`.

These rules are ALWAYS ACTIVE for all notification integration packages, channel adapters, and message formatting modules responsible for processing notification context.

### Rules

- **R-NOTIF-001** MUST_NOT: Notification channel modules MUST NOT define duplicate or conflicting notification context interfaces that bypass @openstatus/notification-base.

### Verify

```bash
# Discover and execute the repository dependency boundary validation check
# Discover and execute the repository build and type verification suite
```

**Accept when:**
- All notification integration modules consume shared notification types, interfaces, or context structures exclusively through @openstatus/notification-base.
- Static analysis and build checks confirm that notification channel adapters do not duplicate baseline notification contracts.

<enforcement>
Claude Code MUST NOT skip or defer verification. Automated dependency analysis and peer review during CI enforce compliance; PRs introducing ad-hoc notification structures without @openstatus/notification-base must be blocked.
</enforcement>