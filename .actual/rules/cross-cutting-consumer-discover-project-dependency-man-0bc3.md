---
version: 2
name: cross-cutting-consumer-discover-project-dependency-man-0bc3
description: "Adoption of @openstatus/notification-base for Notification Packages: Consumer Discover Project Dependency Manifests Lock"
metadata:
  actual:
    adr_id: 0bc350cb-8428-4619-abf1-6734cd48935d
    status: accepted
    candidate_group_id: 76fa8e0d-c217-4490-ba72-6b02bcf6281e
    evidence:
      source: deep
  scope:
    root: packages/notifications/ms-teams
    paths:
      - src/**
---

# Adoption of @openstatus/notification-base for Notification Packages: Consumer Discover Project Dependency Manifests Lock

Scope: applies to `packages/notifications/ms-teams/src/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-NOTIF-001** MUST: The consumer MUST discover the project dependency manifests and lock artifacts to verify that @openstatus/notification-base resolves to a compatible workspace version prior to implementation.

### Verify

```bash
# Discover and execute the repository dependency boundary validation check to verify that notification channel modules consume @openstatus/notification-base.
# Discover and execute the repository build and type verification suite to ensure contract alignment with @openstatus/notification-base.
```

**Accept when:**
- All notification integration modules consume shared notification types, interfaces, or context structures exclusively through @openstatus/notification-base.
- Static analysis and build checks confirm that notification channel adapters do not duplicate baseline notification contracts.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>