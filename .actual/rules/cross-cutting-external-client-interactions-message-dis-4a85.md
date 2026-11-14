---
version: 2
name: cross-cutting-external-client-interactions-message-dis-4a85
description: "Adoption of @openstatus/notification-base for Notification Handlers: External Client Interactions Message Dispatch Handle"
metadata:
  actual:
    adr_id: 4a85e6f3-a6ca-472c-973c-cbbe1c8ce35a
    status: accepted
    candidate_group_id: 5a33e26a-6c2f-476b-9448-c39331b6ce47
    evidence:
      source: deep
  scope:
    root: packages/notifications/twillio-sms
    paths:
      - src/**
---

# Adoption of @openstatus/notification-base for Notification Handlers: External Client Interactions Message Dispatch Handle

Scope: applies to `packages/notifications/twillio-sms/src/**`.

These rules are ALWAYS ACTIVE for notification handler packages and dispatch services responsible for outbound alert delivery across communication channels.

### Rules

- **R-EXT-001** SHOULD: External client interactions for message dispatch SHOULD handle delivery response codes and propagate standardized error states back through the @openstatus/notification-base pipeline.

### Verify

```bash
# Discover and execute the project's static analysis and type verification script from the repository manifest
# Discover and run the project's test runner script to validate notification handler payload parsing and dispatch behavior
```

**Accept when:**
- All notification provider packages successfully import and conform to @openstatus/notification-base contracts with zero static analysis or type errors.
- Automated test suites pass, confirming payload validation and formatted dispatch across all monitor lifecycle states.

<enforcement>
Claude Code MUST NOT skip or defer verification. Continuous integration pipeline, mandatory architectural and code reviews enforce conformance.
</enforcement>