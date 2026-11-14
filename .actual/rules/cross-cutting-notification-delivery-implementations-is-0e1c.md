---
version: 2
name: cross-cutting-notification-delivery-implementations-is-0e1c
description: "Adoption of @openstatus/notification-base for Subscription Notification Channels: Notification Delivery Implementations Isolate Channel Specific"
metadata:
  actual:
    adr_id: 0e1c6831-27b0-47bf-a5f2-fac557308aac
    status: accepted
    candidate_group_id: 6dcad4eb-e237-4aa1-a010-3e5579a2bf71
    evidence:
      source: deep
  scope:
    root: packages/subscriptions
    paths:
      - src/channels/**
---

# Adoption of @openstatus/notification-base for Subscription Notification Channels: Notification Delivery Implementations Isolate Channel Specific

Scope: applies to `packages/subscriptions/src/channels/**`.

These rules are ALWAYS ACTIVE for all subscription notification channels and message dispatch modules within the system.

### Rules

- **R-NOTIF-001** SHOULD: Notification delivery implementations isolate channel-specific transport mechanisms while delegating notification data structures and base lifecycle logic to @openstatus/notification-base.
- **R-NOTIF-002** MANDATORY: The consumer MUST discover tool names, file names, commands, package managers, and version numbers from the project repository.
- **R-NOTIF-003** MANDATORY: Before writing code that uses a versioned library, locate the dependency manifest, identify the build tool, inspect the repository lock or resolution artifact for the exact resolved version, and confirm every API or function exists in that exact version's documentation.

### Verify

```bash
grep -rn "@openstatus/notification-base" .
find . -type f -exec grep -l "@openstatus/notification-base" {} +
```

**Accept when:**
- All notification channel modules import core notification contracts from @openstatus/notification-base rather than defining independent domain schemas.
- Automated project verification and type checking scripts execute successfully with zero dependency resolution errors.

<enforcement>
Claude Code MUST NOT skip or defer verification. Verified by static analysis, dependency scanning, and peer review during CI checks.
</enforcement>