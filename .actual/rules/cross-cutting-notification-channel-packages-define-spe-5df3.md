---
version: 2
name: cross-cutting-notification-channel-packages-define-spe-5df3
description: "Adoption of @openstatus/notification-base for Notification Channel Modules: Notification Channel Packages Define Specific Validation"
metadata:
  actual:
    adr_id: 5df3fac6-85a4-4305-863a-7e2e2a18178f
    status: accepted
    candidate_group_id: d4bf6f6d-dbc1-4ba6-ab0c-83430fda99ff
    evidence:
      source: deep
  scope:
    root: packages/notifications/slack
    paths:
      - src/**
---

# Adoption of @openstatus/notification-base for Notification Channel Modules: Notification Channel Packages Define Specific Validation

Scope: applies to `packages/notifications/slack/src/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-NOTIF-001** MAY: Notification channel packages MAY define channel-specific validation schemas to parse vendor webhook data structures alongside the shared contracts from @openstatus/notification-base.

### Verify

```bash
# Discover workspace dependency graph and verify notification channel packages declare and resolve @openstatus/notification-base
# Discover and execute repository static analysis and typecheck verification routines
```

**Accept when:**
- Static analysis and type checking complete with zero errors across all notification channel packages importing @openstatus/notification-base.
- Dependency graph inspection confirms all notification modules resolve @openstatus/notification-base consistently according to the repository resolution artifact.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>