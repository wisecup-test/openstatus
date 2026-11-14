---
version: 2
name: cross-cutting-incoming-notification-data-payloads-pars-0f0d
description: "@openstatus/notification-base Environment Secrets Handling: Incoming Notification Data Payloads Parsed Validated"
metadata:
  actual:
    adr_id: 0f0d2f27-140c-4984-887d-6c346436c754
    status: accepted
    candidate_group_id: b721e395-9761-4cb0-a0a8-e052244b08ac
    evidence:
      source: deep
  scope:
    root: packages/notifications/telegram
    paths:
      - src/**
---

# @openstatus/notification-base Environment Secrets Handling: Incoming Notification Data Payloads Parsed Validated

Scope: applies to `packages/notifications/telegram/src/**`.

These rules are ALWAYS ACTIVE for notification adapters, dispatch modules integrating with external messaging services, and direct runtime credential resolution for outbound notification requests.

### Rules

- **R-OPEN-001** MUST: Incoming notification data payloads MUST be parsed and validated against defined schemas before processing associated external requests.

### Verify

```bash
# Discover and run the project's static analysis, test validation suites, and code scanners to verify secret handling and input schema compliance
# Ensure no hardcoded secrets or exposed tokens exist in source files
```

**Accept when:**
- All notification test suites pass successfully with environment variables supplied via test harnesses.
- Static analysis reports zero hardcoded secrets or unvalidated payload accesses in notification adapters.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>