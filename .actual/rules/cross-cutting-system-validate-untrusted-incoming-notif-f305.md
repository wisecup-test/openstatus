---
version: 2
name: cross-cutting-system-validate-untrusted-incoming-notif-f305
description: "Zod Runtime Schema Validation for Webhook Notification Boundaries: System Validate Untrusted Incoming Notification Payloads"
metadata:
  actual:
    adr_id: f305e71f-7fbe-46aa-ac94-874cafe0d49a
    status: accepted
    candidate_group_id: 00068acf-f0cb-4824-a15a-a745e550808c
    evidence:
      source: deep
  scope:
    root: packages/notifications/webhook
    paths:
      - src/**
---

# Zod Runtime Schema Validation for Webhook Notification Boundaries: System Validate Untrusted Incoming Notification Payloads

Scope: applies to `packages/notifications/webhook/src/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-ZOD-001** MUST: The system MUST validate all untrusted incoming notification payloads and outgoing external dispatch structures at network boundaries using Zod runtime schemas.

### Verify

```bash
# Discover and execute the repository static analysis and type checking script
# Discover and run the project automated test suite
```

**Accept when:**
- All automated tests covering notification ingestion and dispatch execute with passing results.
- Boundary schema parsing successfully rejects malformed payload structures and permits valid payload objects.

<enforcement>
Claude Code MUST NOT skip or defer verification. Verified by automated continuous integration pipeline executing static type checks and unit test suites, and peer code review.
</enforcement>