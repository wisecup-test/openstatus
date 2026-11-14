---
version: 2
name: cross-cutting-notification-handlers-implementing-opens-44de
description: "Adoption of @openstatus/notification-base for Notification Handlers: Notification Handlers Implementing Openstatus Base Validate"
metadata:
  actual:
    adr_id: 44dee011-39d8-493b-871f-1bf7e7f91657
    status: accepted
    candidate_group_id: 5a33e26a-6c2f-476b-9448-c39331b6ce47
    evidence:
      source: deep
  scope:
    root: packages/notifications/twillio-sms
    paths:
      - src/**
---

# Adoption of @openstatus/notification-base for Notification Handlers: Notification Handlers Implementing Openstatus Base Validate

Scope: applies to `packages/notifications/twillio-sms/src/**`.

These rules are ALWAYS ACTIVE for notification handler packages, dispatch services, and workspace modules implementing provider integrations governed by internal notification contracts.

### Rules

- **R-NOTIF-001** MUST: Notification handlers implementing @openstatus/notification-base MUST validate incoming notification payload data against strict schemas before executing dispatch logic.
- **R-NOTIF-002** MUST: Implement channel-specific notification providers as modular packages extending @openstatus/notification-base, consuming validated notification payloads and mapping them into provider-compatible request structures.
- **R-NOTIF-003** MUST: Ensure all external endpoint secrets and credentials required by notification handlers are injected via environment configuration rather than hardcoded in package source files.

### Verify

```bash
# Discover and execute the project's static analysis and type verification script from the repository manifest to verify @openstatus/notification-base contract conformance.
# Discover and run the project's test runner script to validate notification handler payload parsing and dispatch behavior.
```

**Accept when:**
- All notification provider packages successfully import and conform to @openstatus/notification-base contracts with zero static analysis or type errors.
- Automated test suites pass, confirming payload validation and formatted dispatch across all monitor lifecycle states.

<enforcement>
Claude Code MUST NOT skip or defer verification. Verified by continuous integration pipeline verifying static typing, package boundary conformance, and test passage. Pull requests bypassing @openstatus/notification-base or hardcoding external credentials will be blocked from merging.
</enforcement>