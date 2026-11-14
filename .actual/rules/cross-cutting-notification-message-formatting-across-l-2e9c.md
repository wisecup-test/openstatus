---
version: 2
name: cross-cutting-notification-message-formatting-across-l-2e9c
description: "Adoption of @openstatus/notification-base for Notification Handlers: Notification Message Formatting Across Lifecycle States"
metadata:
  actual:
    adr_id: 2e9cb41f-6ce9-40e1-adf0-a4b90b3d08bf
    status: accepted
    candidate_group_id: 5a33e26a-6c2f-476b-9448-c39331b6ce47
    evidence:
      source: deep
  scope:
    root: packages/notifications/twillio-sms
    paths:
      - src/**
---

# Adoption of @openstatus/notification-base for Notification Handlers: Notification Message Formatting Across Lifecycle States

Scope: applies to `packages/notifications/twillio-sms/src/**`.

These rules are ALWAYS ACTIVE for notification handler packages, dispatch services, and workspace modules implementing provider integrations.

### Rules

- **R-NOTIF-001** SHOULD: Notification message formatting across lifecycle states such as monitor failure, degradation, and recovery SHOULD be encapsulated within the notification dispatch handler adhering to @openstatus/notification-base contracts.
- **R-NOTIF-002** MANDATORY: Implement channel-specific notification providers as modular packages extending @openstatus/notification-base, consuming validated notification payloads and mapping them into provider-compatible request structures.
- **R-NOTIF-003** MANDATORY: Ensure all external endpoint secrets and credentials required by notification handlers are injected via environment configuration rather than hardcoded in package source files.

### Verify

```bash
# Discover and execute the project's static analysis and type verification script from the repository manifest to verify @openstatus/notification-base contract conformance.
# Discover and run the project's test runner script to validate notification handler payload parsing and dispatch behavior.
```

**Accept when:**
- All notification provider packages successfully import and conform to @openstatus/notification-base contracts with zero static analysis or type errors.
- Automated test suites pass, confirming payload validation and formatted dispatch across all monitor lifecycle states.

<enforcement>
Claude Code MUST NOT skip or defer verification. Continuous integration verifies static typing, package boundary conformance, and test passage. Pull requests bypassing @openstatus/notification-base or hardcoding external credentials will be blocked from merging.
</enforcement>