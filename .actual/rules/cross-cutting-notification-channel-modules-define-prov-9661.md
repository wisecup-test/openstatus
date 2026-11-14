---
version: 2
name: cross-cutting-notification-channel-modules-define-prov-9661
description: "Adoption of @openstatus/notification-base for Notification Packages: Notification Channel Modules Define Provider Specific"
metadata:
  actual:
    adr_id: 9661298a-b27c-4283-a7a5-dd0f0870ec03
    status: accepted
    candidate_group_id: 76fa8e0d-c217-4490-ba72-6b02bcf6281e
    evidence:
      source: deep
  scope:
    root: packages/notifications/ms-teams
    paths:
      - src/**
---

# Adoption of @openstatus/notification-base for Notification Packages: Notification Channel Modules Define Provider Specific

Scope: applies to `packages/notifications/ms-teams/src/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-NOTIF-001** MAY: Notification channel modules MAY define provider-specific payload schemas and transport formatting routines to satisfy external client delivery requirements.

### Verify

```bash
# Discover and execute repository dependency boundary validation check
# Discover and execute repository build and type verification suite
```

**Accept when:**
- All notification integration modules consume shared notification types, interfaces, or context structures exclusively through @openstatus/notification-base.
- Static analysis and build checks confirm that notification channel adapters do not duplicate baseline notification contracts.

<enforcement>
Claude Code MUST NOT skip or defer verification. Automated dependency analysis and peer review enforce these rules; violations block merging.
</enforcement>