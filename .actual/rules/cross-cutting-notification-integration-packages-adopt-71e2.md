---
version: 2
name: cross-cutting-notification-integration-packages-adopt-71e2
description: "Adoption of @openstatus/notification-base for Notification Channel Modules: Notification Integration Packages Adopt Import Openstatus"
metadata:
  actual:
    adr_id: 71e2fa83-538c-4c97-875e-f1d135f2c3d2
    status: accepted
    candidate_group_id: d4bf6f6d-dbc1-4ba6-ab0c-83430fda99ff
    evidence:
      source: deep
  scope:
    root: packages/notifications/slack
    paths:
      - src/**
---

# Adoption of @openstatus/notification-base for Notification Channel Modules: Notification Integration Packages Adopt Import Openstatus

Scope: applies to `packages/notifications/slack/src/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-OPENSTATUS-001** MUST: Notification integration packages MUST adopt and import @openstatus/notification-base as the canonical foundation for core notification abstractions, payload interfaces, and shared notification contracts.

### Verify

```bash
# Discover the workspace dependency graph and verify that notification channel packages declare and resolve a dependency on @openstatus/notification-base.
# Discover and execute the repository static analysis and typecheck verification routines to validate that notification channel modules compile cleanly against @openstatus/notification-base.
```

**Accept when:**
- Static analysis and type checking complete with zero errors across all notification channel packages importing @openstatus/notification-base.
- Dependency graph inspection confirms all notification modules resolve @openstatus/notification-base consistently according to the repository resolution artifact.

<enforcement>
Claude Code MUST NOT skip or defer verification. Verified by automated workspace dependency boundary checks, static analysis in continuous integration pipelines, and mandatory peer code review.
</enforcement>