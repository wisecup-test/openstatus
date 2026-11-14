---
version: 2
name: cross-cutting-notification-delivery-implementations-no-740f
description: "Adoption of @openstatus/notification-base for Notification Handlers: Notification Delivery Implementations Not Hardcode External"
metadata:
  actual:
    adr_id: 740f1278-76f2-4c1f-9907-99e712c24e0e
    status: accepted
    candidate_group_id: 5a33e26a-6c2f-476b-9448-c39331b6ce47
    evidence:
      source: deep
  scope:
    root: packages/notifications/twillio-sms
    paths:
      - src/**
---

# Adoption of @openstatus/notification-base for Notification Handlers: Notification Delivery Implementations Not Hardcode External

Scope: applies to `packages/notifications/twillio-sms/src/**`.

These rules are ALWAYS ACTIVE for all notification handler packages, dispatch services, and workspace modules implementing provider integrations governed by internal notification contracts.

### Rules

- **R-NOTIF-001** MUST_NOT: Notification delivery implementations MUST NOT hardcode external client authentication credentials or sensitive endpoints and MUST obtain them from dedicated environment configurations.

### Verify

```bash
# Discover and execute the project's static analysis, type verification, and test runner scripts from the repository manifest.
# (Exact commands must be derived from the repository manifest)
```

**Accept when:**
- All notification provider packages successfully import and conform to @openstatus/notification-base contracts with zero static analysis or type errors.
- Automated test suites pass, confirming payload validation and formatted dispatch across all monitor lifecycle states.

<enforcement>
Verification is mandatory. Claude Code MUST NOT skip or defer verification.
</enforcement>