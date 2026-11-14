---
version: 2
name: cross-cutting-adapters-integrating-openstatus-notifica-9c25
description: "@openstatus/notification-base Environment Secrets Handling: Adapters Integrating Openstatus Notification Base Source"
metadata:
  actual:
    adr_id: 9c258265-c94c-45f4-b3e7-8a63c158f381
    status: accepted
    candidate_group_id: b721e395-9761-4cb0-a0a8-e052244b08ac
    evidence:
      source: deep
  scope:
    root: packages/notifications/telegram
    paths:
      - src/**
---

# @openstatus/notification-base Environment Secrets Handling: Adapters Integrating Openstatus Notification Base Source

Scope: applies to `packages/notifications/telegram/src/**`.

These rules are ALWAYS ACTIVE for notification adapters and dispatch modules integrating with external messaging services, and direct runtime credential resolution for outbound notification requests.

### Rules

- **R-NS-001** MUST: Adapters integrating with @openstatus/notification-base MUST source third-party service authentication tokens directly from runtime process environment variables.

### Verify

```bash
# Discover and run the project's static analysis and test validation suites to verify secret handling and input schema compliance
# Execute the repository code scanner to ensure no hardcoded secrets or exposed tokens exist in source files
```

**Accept when:**
- All notification test suites pass successfully with environment variables supplied via test harnesses.
- Static analysis reports zero hardcoded secrets or unvalidated payload accesses in notification adapters.

<enforcement>
Claude Code MUST NOT skip or defer verification. All pull requests and code modifications must comply with environment variable secret sourcing and pass automated verification checks.
</enforcement>