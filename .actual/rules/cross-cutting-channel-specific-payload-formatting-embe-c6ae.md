---
version: 2
name: cross-cutting-channel-specific-payload-formatting-embe-c6ae
description: "Adoption of @openstatus/notification-base for Notification Modules: Channel Specific Payload Formatting Embed Construction"
metadata:
  actual:
    adr_id: c6aeffaf-657f-4901-b46b-3e438356510a
    status: accepted
    candidate_group_id: c9667bbc-a93e-4be6-8512-d4d9f3810ba7
    evidence:
      source: deep
  scope:
    root: packages/notifications/discord
    paths:
      - src/**
---

# Adoption of @openstatus/notification-base for Notification Modules: Channel Specific Payload Formatting Embed Construction

Scope: applies to `packages/notifications/discord/src/**`.

These rules are ALWAYS ACTIVE for all notification integration modules and dispatchers within the notification subsystem.

### Rules

- **R-NOTIF-001** SHOULD: Channel-specific payload formatting and embed construction SHOULD isolate provider nuances while referencing base notification schemas.
- **R-NOTIF-002** MANDATORY: Consumers must align data transformation boundaries with the schemas and interfaces exposed by the base notification package, and notification provider implementations must isolate provider-specific payload formatting from shared notification domain logic.
- **R-NOTIF-003** MANDATORY: Before writing code that uses a versioned library, execute in order: find the dependency manifest, identify the build tool, inspect the repository lock or resolution artifact for the exact resolved version, look up official documentation for that exact version, confirm every API exists in that version, and re-run for version-sensitive behavior.

### Verify

```bash
# Discover and execute repository dependency boundary and module linting scripts.
# Discover and run test suites targeting notification provider modules.
```

**Accept when:**
- Static analysis confirms notification integration modules import shared notification primitives from @openstatus/notification-base.
- All automated test suites and contract validations for notification delivery pass without structural dependency violations.

<enforcement>
Claude Code MUST NOT skip or defer verification. Continuous integration dependency checks and automated module boundary validation rules enforce these policies.
</enforcement>