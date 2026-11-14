---
version: 2
name: cross-cutting-notification-channel-modules-extend-shar-1ef4
description: "Adoption of @openstatus/notification-base for Subscription Notification Channels: Notification Channel Modules Extend Shared Definitions"
metadata:
  actual:
    adr_id: 1ef4cad9-8f9c-46bb-80d6-c8f2fbd45c80
    status: accepted
    candidate_group_id: 6dcad4eb-e237-4aa1-a010-3e5579a2bf71
    evidence:
      source: deep
  scope:
    root: packages/subscriptions
    paths:
      - src/channels/**
---

# Adoption of @openstatus/notification-base for Subscription Notification Channels: Notification Channel Modules Extend Shared Definitions

Scope: applies to `packages/subscriptions/src/channels/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-NOTIF-001** MAY: Notification channel modules MAY extend shared definitions with channel-specific delivery options when transport protocols require additional metadata.

### Verify

```bash
grep -rn "@openstatus/notification-base" .
find . -type f -exec grep -l "@openstatus/notification-base" {} +
```

**Accept when:**
- All notification channel modules import core notification contracts from @openstatus/notification-base rather than defining independent domain schemas.
- Automated project verification and type checking scripts execute successfully with zero dependency resolution errors.

<enforcement>
Claude Code MUST NOT skip or defer verification. All notification channel implementations must derive payloads from the shared module @openstatus/notification-base.
</enforcement>