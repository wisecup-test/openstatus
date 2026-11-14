---
version: 2
name: cross-cutting-notification-client-integrations-not-exp-b97d
description: "@openstatus/notification-base Environment Secrets Handling: Notification Client Integrations Not Expose Secret"
metadata:
  actual:
    adr_id: b97d59cf-cb9a-44eb-bf66-438892daf376
    status: accepted
    candidate_group_id: b721e395-9761-4cb0-a0a8-e052244b08ac
    evidence:
      source: deep
  scope:
    root: packages/notifications/telegram
    paths:
      - src/**
---

# @openstatus/notification-base Environment Secrets Handling: Notification Client Integrations Not Expose Secret

Scope: applies to `packages/notifications/telegram/src/**`.

These rules are ALWAYS ACTIVE for notification adapters, dispatch modules integrating with external messaging services, and direct runtime credential resolution for outbound notification requests.

### Rules

- **R-NOTIF-001** MUST_NOT: Notification client integrations MUST NOT expose secret credentials in persistent logs, error traces, or unencrypted storage.
- **R-NOTIF-002** MANDATORY: Execute lock-version grounding (find manifest, identify build tool, inspect lock/resolution artifact, lookup official docs for exact version, confirm API existence) before writing code using a versioned library.
- **R-NOTIF-003** MANDATORY: Verify that all required environment variables are defined in the runtime deployment configuration prior to service initialization.
- **R-NOTIF-004** MANDATORY: Ensure outbound request builders sanitize sensitive parameters before emitting diagnostic traces.

### Verify

```bash
# Discover and run the project's static analysis and test validation suites
# Execute the repository code scanner to ensure no hardcoded secrets or exposed tokens exist in source files
```

**Accept when:**
- All notification test suites pass successfully with environment variables supplied via test harnesses.
- Static analysis reports zero hardcoded secrets or unvalidated payload accesses in notification adapters.

<enforcement>
Claude Code MUST NOT skip or defer verification. Verification is mandatory via automated CI checks and peer code reviews.
</enforcement>