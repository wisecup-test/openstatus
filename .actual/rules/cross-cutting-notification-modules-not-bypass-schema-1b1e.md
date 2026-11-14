---
version: 2
name: cross-cutting-notification-modules-not-bypass-schema-1b1e
description: "Zod Schema Definition for Notification Payloads: Notification Modules Not Bypass Schema Validation"
metadata:
  actual:
    adr_id: 1b1e4874-5f0d-4fb2-afe1-2f8b998b2bd8
    status: accepted
    candidate_group_id: 34b9637f-6219-4ef8-9a08-9b44f389c821
    evidence:
      source: deep
  scope:
    root: packages/notifications/opsgenie
    paths:
      - src/**
---

# Zod Schema Definition for Notification Payloads: Notification Modules Not Bypass Schema Validation

Scope: applies to `packages/notifications/opsgenie/src/**`.

These rules are ALWAYS ACTIVE for notification integration adapters constructing alert payloads for external dispatch.

### Rules

- **R-NOTIF-001** MUST_NOT: Notification modules MUST_NOT bypass schema validation by transmitting raw or unvalidated objects directly to external notification endpoints.

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