---
version: 2
name: cross-cutting-notification-delivery-services-packages-fbfa
description: "Adoption of @openstatus/notification-base for Notification Handlers: Notification Delivery Services Packages Implement Dispatch"
metadata:
  actual:
    adr_id: fbfac542-8865-4c3d-a666-f54146f55250
    status: accepted
    candidate_group_id: 5a33e26a-6c2f-476b-9448-c39331b6ce47
    evidence:
      source: deep
  scope:
    root: packages/notifications/twillio-sms
    paths:
      - src/**
---

# Adoption of @openstatus/notification-base for Notification Handlers: Notification Delivery Services Packages Implement Dispatch

Scope: applies to `packages/notifications/twillio-sms/src/**`.

These rules are ALWAYS ACTIVE for notification handler packages and dispatch services responsible for outbound alert delivery across communication channels, and workspace modules implementing provider integrations governed by internal notification contracts.

### Rules

- **R-NOTIF-001** MUST: Notification delivery services and packages MUST implement notification dispatch workflows by importing and extending the @openstatus/notification-base module.
- **R-NOTIF-002** MANDATORY (Discovery Policy): The consumer MUST derive all tool names, file names, commands, package managers, and version numbers from the project repository.
- **R-NOTIF-003** MANDATORY (Lock-Version Grounding): Before writing code that uses a versioned library, execute in order: (1) Find dependency manifest, (2) Identify build tool, (3) Inspect repository lock/resolution artifact, (4) Look up official documentation for that exact version, (5) Confirm every API/class/function exists in that version, (6) Re-run per dependency at point of use.
- **R-NOTIF-004** MUST: Implement channel-specific notification providers as modular packages extending @openstatus/notification-base, consuming validated notification payloads and mapping them into provider-compatible request structures.
- **R-NOTIF-005** MUST: Ensure all external endpoint secrets and credentials required by notification handlers are injected via environment configuration rather than hardcoded in package source files.

### Verify

```bash
# Discover and execute the project's static analysis and type verification script from the repository manifest
# Discover and run the project's test runner script to validate notification handler payload parsing and dispatch behavior
```

**Accept when:**
- All notification provider packages successfully import and conform to @openstatus/notification-base contracts with zero static analysis or type errors.
- Automated test suites pass, confirming payload validation and formatted dispatch across all monitor lifecycle states.

<enforcement>
Claude Code MUST NOT skip or defer verification. Continuous integration verifies static typing, package boundary conformance, and test passage.
</enforcement>