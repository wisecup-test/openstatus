---
version: 2
name: cross-cutting-subscription-notification-channel-module-bee2
description: "Adoption of @openstatus/notification-base for Subscription Notification Channels: Subscription Notification Channel Modules Adopt Consume"
metadata:
  actual:
    adr_id: bee23af3-319f-4279-808c-ff25cf29a93d
    status: accepted
    candidate_group_id: 6dcad4eb-e237-4aa1-a010-3e5579a2bf71
    evidence:
      source: deep
  scope:
    root: packages/subscriptions
    paths:
      - src/channels/**
---

# Adoption of @openstatus/notification-base for Subscription Notification Channels: Subscription Notification Channel Modules Adopt Consume

Scope: applies to `packages/subscriptions/src/channels/**`.

These rules are ALWAYS ACTIVE for all subscription notification channels and message dispatch modules within the system.

### Rules

- **R-SUB-001** MUST: Subscription notification channel modules MUST adopt and consume shared notification abstractions from @openstatus/notification-base for standardizing notification payloads and dispatch contracts across delivery channels.

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