---
version: 2
name: cross-cutting-notification-delivery-modules-not-constr-4b71
description: "Adoption of @openstatus/notification-base for Subscription Notification Channels: Notification Delivery Modules Not Construct Divergent"
metadata:
  actual:
    adr_id: 4b71e27d-c58c-4811-8e60-6f69d759d99f
    status: accepted
    candidate_group_id: 6dcad4eb-e237-4aa1-a010-3e5579a2bf71
    evidence:
      source: deep
  scope:
    root: packages/subscriptions
    paths:
      - src/channels/**
---

# Adoption of @openstatus/notification-base for Subscription Notification Channels: Notification Delivery Modules Not Construct Divergent

Scope: applies to `packages/subscriptions/src/channels/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-NOTIF-001** MUST_NOT: Notification delivery modules MUST NOT construct divergent notification payload contracts or reimplement abstractions provided by @openstatus/notification-base.

### Verify

```bash
grep -rn "@openstatus/notification-base" .
find . -type f -exec grep -l "@openstatus/notification-base" {} +
```

**Accept when:**
- All notification channel modules import core notification contracts from @openstatus/notification-base rather than defining independent domain schemas.
- Automated project verification and type checking scripts execute successfully with zero dependency resolution errors.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>