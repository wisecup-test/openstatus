---
version: 2
name: cross-cutting-notification-integration-modules-define-7d0a
description: "Zod Schema Definition for Notification Payloads: Notification Integration Modules Define External Alert"
metadata:
  actual:
    adr_id: 7d0aec75-477b-4918-982a-71546a0a609e
    status: accepted
    candidate_group_id: 34b9637f-6219-4ef8-9a08-9b44f389c821
    evidence:
      source: deep
  scope:
    root: packages/notifications/opsgenie
    paths:
      - src/**
---

# Zod Schema Definition for Notification Payloads: Notification Integration Modules Define External Alert

Scope: applies to `packages/notifications/opsgenie/src/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-NOT-001** MUST: Notification integration modules MUST define external alert payload schemas using Zod object schema declarations to validate payload structure prior to provider dispatch.

### Verify

```bash
# Discover the repository test runner from project configuration and execute notification payload schema unit tests.
# Discover the project static type analysis runner and execute type validation across notification integration packages.
```

**Accept when:**
- All notification payload validation tests pass with zero schema parsing failures.
- Type checking completes with zero diagnostic errors across all schema definitions and inferred types.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>